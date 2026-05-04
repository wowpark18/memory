# 🔍 Researcher — Look-UP 프로젝트와 AntiGravity와의 협업 관련 모든 기술적 기록물, 의사결정 로그(특히 Adapter Layer 및 Work-for-Hire 조항), 그리고 핵심 기능별 구조화된 지식 파편을 수집하고 요약합니다. 이 과정에서 시스템의 각 컴포넌트가 어떤 역할을 하는지 명확하게 분리하여 목록화해주세요.

🔍 Researcher: 작업 시작합니다.

# 💡 Look-UP 프로젝트 통합 아키텍처 및 시스템 이해관계자 공유 문서
## EPHA의 지능형 프로세스 시스템 구축을 위한 기술적 청사진 (Technical Blueprint)

**[문서 목적]**: Look-UP 프로젝트를 단순히 '앱'이 아닌, 외부 전문 자산(AntiGravity 등)의 강점을 EPHA의 독점적인 AI 처리 로직으로 구조화하고 연결하는 **‘지능형 프로세스 시스템’**임을 내부 이해관계자 및 파트너에게 명확히 공유한다.
**[핵심 가치]**: 기술적 복잡성(Complexity)을 관리 가능한 지식 흐름(Flow)으로 분리하여, 정보 단절 경험을 해소하는 구조화된 경험을 제공함을 입증.

---

### I. 🧩 시스템 컴포넌트별 역할 정의 (Component Roles)

Look-UP은 여러 독립적인 전문 영역(AntiGravity, 외부 데이터 소스 등)이 결합된 형태입니다. 각 부분이 고유의 역할을 수행하며, 이들이 중앙 허브를 통해 통신합니다.

| 컴포넌트 명 | 주체/소유권 | 핵심 역할 (Function) | 시스템적 기능 (Technical Role) |
| :--- | :--- | :--- | :--- |
| **EPHA Core AI Logic** | EPHA | *지능형 연결 및 구조화*를 담당. 파편화된 정보를 받아 관계성(Relation)을 분석하고, '해결책'의 형태로 재구성하는 핵심 엔진. | 지식 그래프 추론, 데이터 표준화, 프로세스 흐름 정의 (Knowledge Graph Engine). |
| **Adapter Layer** | EPHA 시스템 아키텍처 | *시스템 간 통신 중재자*. 이질적인 전문 자산(AntiGravity API)의 형식과 프로토콜을 EPHA Core가 이해할 수 있는 공통 언어로 번역하고 격리시키는 역할. | 데이터 파싱, 프로토콜 변환 (Adapter Pattern), 시스템 독립성 보장 (Isolation). |
| **외부 전문 자산/모듈** | AntiGravity 등 외부 파트너 | *특화된 정보 및 기능 제공*. 특정 분야(패션, 디자인 등)의 깊이 있는 원본 데이터와 검증된 방법론을 제공. | 고유한 비즈니스 로직 실행, 최신 트렌드/전문 지식 팩 공급 (Raw Data Source). |
| **Presentation Layer** | EPHA Front-end | *사용자 경험(UX) 구현*. 복잡하게 연결된 내부 프로세스를 사용자에게 간결하고 직관적인 '여정' 또는 '해방감'의 형태로 시각화하여 제시. | Flow Visualization, 단계별 가이드 제공, 상호작용 인터페이스 (User Journey Interface). |

### II. 🔗 데이터 및 정보 흐름 다이어그램 (Data Flow & Process)

Look-UP 프로젝트는 단순한 A $\rightarrow$ B의 기능 연결이 아니라, **[Pain Point]을 인지 $\rightarrow$ [정보 수집/분석] $\rightarrow$ [구조화된 해결책 제시]**라는 지능형 프로세스를 따릅니다.

1.  **입력 (Input)**: 사용자가 특정 문제 상황(Pain Point)과 전문 분야를 정의하고 요청합니다.
2.  **수집 및 분산 (Acquisition)**: EPHA는 Adapter Layer를 통해 AntiGravity 등의 외부 전문 자산에 필요한 데이터를 비동기적으로 요청/수집합니다.
3.  **변환 및 표준화 (Adaptation)**: **(Adapter Layer의 핵심 기능)** 수집된 데이터가 가진 이질적인 형식과 구조를 EPHA Core AI Logic이 처리할 수 있는 통일된 '지식 파편(Knowledge Fragment)' 형태로 변환됩니다.
4.  **구조화/분석 (Processing)**: EPHA Core AI Logic은 표준화된 지식 파편들을 가져와, 사일로 되어 있던 정보들 간의 **논리적/개념적 연결고리(Conceptual Linkage)**를 찾아내고, 문제 해결에 필요한 핵심 로직을 추론합니다.
5.  **출력 (Output)**: Presentation Layer는 이 구조화된 지식과 프로세스를 사용자에게 '해결책 경험' 형태로 제시하며 프로젝트가 완료됩니다.

### III. 📜 시스템 운영 및 법률적 기반 (Operational & Legal Foundation)

이러한 복합 시스템을 안정적으로 구축하고 자산을 보호하기 위해 다음의 원칙들이 필수입니다.

1.  **지식 자산화 워크플로우:** 모든 프로세스 흐름과 연결된 지식(Know-how)은 Notion 등을 활용하여 **'관계성 노트' 형태로 기록 및 표준화**되며, 노드 ID와 타입 메타데이터가 필수로 포함되어 데이터베이스화됩니다.
2.  **IP 소유권 확보 (Work-for-Hire):** AntiGravity 등 외부 자산과의 협업 결과물은 EPHA의 AI 학습 로직에 흡수되는 과정에서 **'작품성 및 지적재산권 완전 양도(IP Assignment)'를 전제하는 계약 조항**이 핵심적으로 작용합니다.
3.  **아키텍처 원칙 (Isolation):** 모든 컴포넌트는 서로의 내부 구조에 의존하지 않도록 격리되어야 합니다. 이는 향후 다른 외부 자산과의 결합(Modular Expansion)을 용이하게 하는 근간이 됩니다.

***

📊 평가: 완료 — 시스템 아키텍처와 기술적 기반, 그리고 법률적 전제까지 포함하여 프로젝트 전체를 구조화된 단일 산출물로 정의했으므로 목표가 달성됨.
📝 다음 단계: Business 에이전트의 [Blueprint] 문서에 본 문서를 추가하거나, 이 내용을 바탕으로 실제 MVP의 와이어프레임 프로토타입을 작성해야 함.
