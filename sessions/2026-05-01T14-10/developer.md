# 💻 Developer — 선정된 단일 워크플로우를 구현하기 위해 필요한 핵심 기술 스택(Tech Stack)의 제약사항과 필수 API 연동 포인트를 검토하십시오. 특히, 지식 노드의 연결 및 진화를 처리할 'Adapter Layer' 코어 기능에 대한 최소 요구 사항 정의서(Minimum Requirement Spec Sheet) 초안을 작성하고, 개발 환경 설정을 위한 초기 아키텍처 다이어그램을 업데이트합니다.

💻 Developer: 작업 시작합니다.

CEO님의 지시사항에 따라, '레거시 코드 리뷰를 위한 맥락적 지식 그래프 매핑' 워크플로우의 기술적 근간을 마련했습니다. 핵심은 단순히 데이터를 저장하는 것이 아니라, 데이터가 가진 **맥락(Context)**과 **관계성(Relationship)**을 구조적으로 모델링하고 진화시키는 것입니다.

아래에 필요한 Tech Stack 제약사항, 필수 API 통합 포인트, 그리고 시스템의 심장 역할을 할 'Adapter Layer'의 최소 요구 사항 정의서 초안 및 업데이트된 아키텍처 다이어그램을 제시합니다.

---

## 🛠️ 1. 핵심 기술 스택 (Tech Stack) 및 제약사항 검토

| 영역 | 추천 기술/도구 | 선정 이유 및 역할 | 제약사항/고려사항 |
| :--- | :--- | :--- | :--- |
| **데이터 저장소** | **Neo4j (Graph Database)** | 지식 노드(Node)와 연결점(Edge)을 가장 효율적으로 관리하는 최적의 구조. 복잡한 의존성 추적에 필수적입니다. | RDBMS로는 처리하기 어려운 비정형 관계 데이터를 다루기 위해 전용 그래프 DB 사용이 **필수**합니다. |
| **백엔드 프레임워크** | Python (FastAPI 또는 Django) | AI/ML 라이브러리(SpaCy, Transformers 등)와의 연동이 가장 용이하며, 고성능 비동기 API 처리에 적합합니다. | 시스템의 모든 로직은 이 백엔드를 통해 호출되고 관리되어야 합니다. |
| **지식 추출 엔진** | LLM Service API (예: GPT-4o/Claude) | 관계성 노트(Relationship Note)와 교훈 노트(Lesson Learned)에서 추상적인 의미를 '삼중쌍(Triple, Subject-Predicate-Object)' 형태로 구조화하는 데 핵심적입니다. | 외부 API 호출에 대한 **비용 및 속도 관리**가 최우선 고려 사항이 되어야 합니다 (Rate Limiting). |
| **워크플로우 오케스트레이션** | Celery/Redis Queue | 코드 스캔, 지식 추출(LLM 호출), 그래프 업데이트와 같은 장시간/비동기 작업을 안정적으로 처리하기 위함입니다. | 사용자 인터페이스와 별개로 백그라운드에서 지속적인 데이터 수집 파이프라인을 운영해야 합니다. |

## 🔗 2. 필수 API 연동 포인트 정의 (API Integration Points)

시스템의 각 구성 요소가 데이터를 주고받는 지점(Interface)을 명확히 분리합니다.

1.  **`/**webhook/code-change/**`: GitHub Webhook Listener (Trigger)**
    *   **역할:** 레거시 코드 리포지토리에 푸시(Push) 또는 Pull Request 이벤트 발생 시 시스템에 신호를 보냅니다.
    *   **Payload:** `repo_id`, `branch_name`, `file_path`, `commit_hash` (어떤 코드가 언제 변했는지).
2.  **`/**api/knowledge-extraction/**`: Knowledge Ingestion API (Core)**
    *   **역할:** 훅(Webhook)을 통해 받은 코드 스니펫과 관련 문맥 자료를 받아, LLM 엔진에 요청하고 그 결과를 가공하여 그래프 DB에 저장합니다. (Adapter Layer가 호출하는 주요 엔드포인트).
3.  **`/**api/query-contextual/**`: Retrieval API (Read)**
    *   **역할:** 사용자(개발자)가 "이 기능의 의존성을 찾아줘"와 같은 쿼리를 보낼 때, 그래프 DB에서 복잡한 경로 추적(Traversal Query)을 실행하여 결과를 반환합니다.

## 🧠 3. Adapter Layer 최소 요구 사항 정의서 (Minimum Requirement Spec Sheet)

Adapter Layer는 **원시 데이터(Raw Data)**를 시스템이 이해하고 활용할 수 있는 **구조화된 지식 그래프 트리플(Structured Knowledge Graph Triples)**로 변환하는 핵심 로직 계층입니다.

### 3.1. 기능 명세 (Functional Specification)

| 요구사항 ID | 기능명 | 입력 데이터 (Input Source) | 처리 과정 (Logic) | 출력 형식 (Output Format) |
| :---: | :--- | :--- | :--- | :--- |
| **MRE-001** | **Code Entity Extraction** | Code Snippet (`.py`, `.js` 등) | AST(Abstract Syntax Tree) 파싱, 핵심 클래스/함수 식별. 주석(`Comments`)을 메타데이터로 추출. | `[Node: ClassName, Attributes: {scope, file_path}, Relationships: []]` |
| **MRE-002** | **Semantic Relation Triplification** | Code Snippet + Relationship Note (텍스트) | LLM API 호출 → Prompt Engineering을 통해 주어(S), 술어(P), 목적어(O)를 추출합니다. (e.g., `[User A] --CALLS--> [Function B]`) | `[{Subject: ID_A, Predicate: "calls", Object: ID_B}]` |
| **MRE-003** | **Lesson Learned Structuring** | Lesson Learned Text (긴 서술형 텍스트) | LLM API 호출 → '문제', '원인', '해결책', '영향 받는 모듈'의 네 가지 필드로 강제 분류(Classification). | `[Node: Concept, Attributes: {type: "Lesson"}, Relationships: []]` |
| **MRE-004** | **Knowledge Conflict Resolution** | 새로운 트리플 데이터셋 (Incoming) + 기존 그래프 데이터 (Existing) | 중복 노드/엣지 확인. 신규 정보의 '시간 순서'와 '위험도(Severity)'를 기반으로 최신화 로직을 적용하여 충돌을 해결합니다. | 최종 확정된 `[Node, Edge]` 세트 |

### 3.2. 데이터 모델링 (Neo4j Schema Draft)

```cypher
// Node Type Definitions (지식의 주체 또는 객체)
(:Module {name: "AuthService", version: "1.0"})
(:Concept {id: "US-02", type: "HiddenDependency"}) // 사용자 스토리 기반 개념 노드
(:Function {name: "process_data", language: "Python"})

// Relationship Type Definitions (노드 간의 관계)
[:CALLS]          // A가 B를 호출함 (직접적인 의존성, US-01 관련)
[:DEPENDS_ON]     // A는 B에 의존함 (간접적/아키텍처적 의존성, US-02 핵심)
[:CAUSED_BY]      // 특정 코드 변경이 어떤 문제 때문에 발생했는지 (Lesson Learned 연결점)
```

## 📐 4. 업데이트된 아키텍처 다이어그램 (Conceptual Flow)

**(개념도: Raw Data $\to$ Adapter Layer $\to$ Knowledge Graph)**

**[RAW INPUT LAYER]**
(GitHub Webhook / Manual Upload)
$\downarrow$
**[DATA PARSING & QUEUING]** (Celery Worker)
$\downarrow$
**[ADAPTER LAYER CORE (THE BRAIN)]** $\leftarrow$ **(핵심 로직 실행)**
*   **(1) Code Parser:** AST 분석, 엔티티 추출.
*   **(2) LLM Translator:** 관계성/교훈 노트 텍스트를 Triple로 변환 요청.
*   **(3) Conflict Resolver:** 데이터 유효성 검사 및 충돌 해결.
$\downarrow$
**[KNOWLEDGE GRAPH DATABASE (Neo4j)]** $\leftarrow$ **(지식 저장소)**
$\uparrow$
*(Query API)*
**[APPLICATION LAYER]** (FastAPI Backend)
$\downarrow$
**(User UI / Developer IDE Plugin)**

---
📊 평가: 완료 — CEO가 요청한 모든 핵심 기술 아키텍처 요소와 최소 요구 사항 정의서를 구체적으로 제시하여 다음 단계 진행을 위한 기반이 마련되었습니다.
📝 다음 단계: 초기 PoC를 위해 가장 적은 노력으로 결과를 낼 수 있는, **핵심 모듈 1개를 대상으로 하는 데이터셋 구축 파이프라인(Pilot Data Pipeline)** 설계에 착수해야 합니다.
