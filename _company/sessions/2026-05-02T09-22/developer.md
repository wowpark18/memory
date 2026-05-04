# 💻 Developer — 핵심 기능인 '예측 및 개입 AI 진단 프로세스'의 기술적 상세 설계(Technical Blueprint)를 완성하세요. 사용자의 입력 데이터 수집 단계부터, 노드 연결 그래프 기반으로 정보를 처리하고 최종적인 경고/개입 메시지를 출력하는 전체 시스템 아키텍처 다이어그램(Flowchart 포함)과 핵심 API 엔드포인트 스펙을 정의해야 합니다.

💻 Developer: 작업 시작합니다.

## 🛠️ [TECHNICAL BLUEPRINT] 예측 및 개입 AI 진단 프로세스 설계 완료
**목표:** 사용자의 입력 데이터를 수집하여 지식 노드 연결 그래프(Knowledge Graph) 기반으로 분석하고, 최종적인 '개입 메시지'를 출력하는 End-to-End 시스템 아키텍처와 API 스펙을 정의합니다. 이 청사진은 PoC 개발의 기술적 기준점(Technical Benchmark)이 됩니다.

---

### 1. 시스템 아키텍처 다이어그램 (Flowchart 기반 설명)
**기술 원칙:** 데이터가 '정보 단절' 상태에서 '지식 연결 및 해소 과정'을 거쳐 최종적으로 '행동 변화 유도'로 이어지는 흐름에 초점을 맞춥니다.

```mermaid
graph TD
    A[사용자 입력/데이터 수집] --> B(Data Validation & Normalization);
    B --> C{Event Stream Queue (Kafka / PubSub)};
    C --> D[Knowledge Graph Ingestion Module];
    D --> E(Graph Database: Neo4j 등);
    E --> F{Relationship Scoring Engine};

    subgraph Predictive Diagnosis Core
        F --> G[Pattern Matching & Anomaly Detection];
        G --> H(Predictive Model Layer - ML/AI);
        H --> I{Intervention Logic Rule Set};
    end

    I --> J{Diagnosis Result: Gap Found?};
    J -- YES (단절 감지) --> K[개입 메시지 생성 모듈];
    J -- NO (정상 흐름) --> L[Process Flow Logging];
    K --> M(API Gateway / Output);
    L --> M;

    M --> N[Frontend Display: 경고/Tip 제공];
```

**단계별 설명:**
1.  **데이터 수집 및 정규화 (A $\rightarrow$ B):** 사용자가 OCR, 폼 입력, 또는 외부 API를 통해 데이터를 제출합니다. 이 데이터는 즉시 유효성 검사(Schema Validation)와 표준 형식으로 변환됩니다.
2.  **이벤트 스트림 (C):** 모든 원본 데이터는 비동기 처리를 위해 메시지 큐에 적재되어 시스템 부하 분산 및 재처리 가능성을 확보합니다.
3.  **지식 그래프 주입 (D $\rightarrow$ E):** 전문 모듈은 들어온 데이터를 분석하여 기존의 노드(Node)와 관계(Relationship)로 정의하고 Graph DB에 저장/업데이트합니다.
4.  **진단 코어 작동 (F $\rightarrow$ I):** 시스템은 단순히 연결된 정보를 보여주는 것을 넘어, **최단 경로 탐색 기반으로 '정보 흐름의 비효율성'이나 '누락된 지식 간의 단절(Gap)'**을 찾아냅니다. 이 Gap이 곧 진단의 핵심입니다.
5.  **개입 및 출력 (K $\rightarrow$ N):** Gap이 발견되면, 사전에 정의된 `Intervention Logic Rule Set`에 따라 가장 효과적이고 사용자에게 공감적인 톤의 경고/팁 메시지를 생성하고 프론트엔드에 전달합니다.

---

### 2. 핵심 API 엔드포인트 스펙 (API Specification)
**기술 스택:** 백엔드는 Node.js (Express/NestJS), 데이터베이스는 Neo4j 기반 Graph DB를 사용한다고 가정합니다. 모든 호출은 `Bearer Token` 인증을 거쳐야 합니다.

#### A. 데이터 입력 및 그래프 업데이트 API
*   **목적:** 새로운 원시 데이터를 시스템에 제출하고, 이를 지식 그래프에 통합하여 관계성을 구축하는 초기 단계.
*   **엔드포인트:** `POST /api/v1/data/ingest`
*   **요청 본문 (Request Body Schema):**
    ```json
    {
      "user_id": "string",            // 사용자 고유 ID (필수)
      "source_type": "enum[OCR|FORM|API]", // 데이터 출처 타입
      "raw_data": {                   // 원시 데이터를 구조화한 객체
        "text_content": "string",
        "metadata": {                 // 이미지 파일 경로, 타임스탬프 등 부가 정보
          "timestamp": "ISO 8601 string",
          "source_ip": "string"
        }
      },
      "keywords": ["array of strings"] // 추출된 핵심 키워드 (Node 후보)
    }
    ```
*   **응답 본문 (Response Body Schema):**
    ```json
    {
      "status": "SUCCESS",
      "message": "Data ingested and graph updated successfully.",
      "processed_nodes": ["list of nodes created/updated"],
      "new_relationships_count": 5, // 이번 입력으로 추가된 관계 개수 (측정 지표)
      "ingestion_timestamp": "ISO 8601 string"
    }
    ```

#### B. 진단 및 예측 요청 API (Diagnosis Core Trigger)
*   **목적:** 현재까지의 사용자의 모든 기록과 데이터를 기반으로, 가장 높은 위험도를 가진 '정보 단절' 지점과 그 해결책을 요청합니다. (이것이 핵심 비즈니스 로직 호출입니다.)
*   **엔드포인트:** `GET /api/v1/diagnosis/{user_id}`
*   **쿼리 파라미터 (Query Parameters):**
    ```
    ?depth=3            // 탐색할 그래프의 깊이 (예: 3단계 연결까지 분석)
    &focus_area=finance // 진단을 집중할 영역 (선택적, 예: 재무 관리)
    ```
*   **응답 본문 (Response Body Schema):**
    ```json
    {
      "status": "DIAGNOSIS_COMPLETE",
      "user_id": "string",
      "analysis_timestamp": "ISO 8601 string",
      "diagnosis_summary": {
        "overall_risk_score": 0.78, // 0.0 (안전) ~ 1.0 (위험)
        "criticality_level": "HIGH", // LOW | MEDIUM | HIGH
        "key_gap_identified": "세금 신고 시점과 관련 재무 자료의 연결성 단절" // 발견된 가장 큰 문제점 설명
      },
      "intervention_recommendations": [ // 개입 메시지 리스트
        {
          "severity": "CRITICAL",
          "title": "🚨 경고: 데이터 누락으로 인한 잠재적 재정 위험 발생",
          "description": "지난 분기 보고서의 '인건비 변동' 데이터가 현재 세금 신고 프로세스에 연결되지 않아, 실제 비용 처리 시 누락될 수 있습니다.",
          "suggested_action": "HR 시스템에서 인건비 상세 데이터를 추출하여 [세무처리] 노드와 직접 연결하십시오." // 사용자에게 제시할 구체적 액션
        },
        // ... 기타 추천 사항들
      ],
      "related_nodes": ["node_id1", "node_id2"] // 해결에 필요한 관련 지식 노드 ID 목록
    }
    ```

---

📊 평가: 완료 — CEO의 요구사항을 충족하는 기술적 상세 설계와 실행 가능한 API 스펙을 제공하여, 다음 개발 단계(PoC 구현)를 위한 청사진이 완성되었기 때문입니다.
📝 다음 단계: 이 사양서(`Technical Blueprint`)를 기반으로 백엔드 초기 구조체 및 핵심 데이터 모델링 코드를 작성하고 테스트할 환경 구축에 착수해야 합니다.
