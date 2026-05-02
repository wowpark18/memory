# 💻 Developer — 와이어프레임 기반의 '지식 연결 시각화' 기능을 구현하기 위한 최소 기능 API 엔드포인트 구조 초안을 작성해주세요. 특히, 노드 ID와 타입 메타데이터를 받아 그래프 형태로 관계성을 추적하고 이를 JSON 응답으로 반환하는 백엔드 로직 뼈대(Pseudo Code)가 필요합니다.

💻 Developer: 작업 시작합니다.

## 🛠️ EPHA 지식 연결 시각화 기능 구현을 위한 핵심 API 엔드포인트 설계

요청하신 '지식 연결 시각화' 기능을 백엔드에서 처리하기 위한 최소한의 API 구조와 Pseudo Code를 작성했습니다. 이 로직은 단순히 데이터를 받는 것이 아니라, 입력된 노드들 간의 잠재적 관계성(Connection)을 분석하고 그 결과를 그래프 라이브러리가 소비할 수 있는 표준 JSON 형태로 반환하는 것을 목표로 합니다.

### 1. API 명세 (API Specification)

| 항목 | 내용 |
| :--- | :--- |
| **엔드포인트** | `/api/v1/knowledge/connect` |
| **HTTP Method** | `POST` |
| **요청 본문 (Request Body)** | JSON Object |
| **성공 응답 코드** | `200 OK` |
| **실패 응답 코드** | `400 Bad Request`, `500 Internal Server Error` |

---

### 2. 데이터 구조 정의 (Schema Definition)

#### A. 요청 본문 (`Input Payload`)

데이터 파편(Fragmented Data)을 의미하는 여러 노드들을 배열 형태로 전송받습니다.

```json
{
  "source_nodes": [
    {
      "node_id": "uuid-1234",
      "node_type": "Concept",       // 예: 'Concept', 'SourceDoc', 'UserNote'
      "metadata": {                  // 노드에 대한 메타데이터 (저자, 날짜 등)
        "source": "user_input",
        "date": "2026-05-02"
      },
      "content_snippet": "AI는 단순 검색을 넘어 프로세스 자체를 상품화해야 한다." // 실제 텍스트 내용의 일부
    },
    {
      "node_id": "uuid-5678",
      "node_type": "KeywordCluster",  // 예: 'KeywordCluster', 'IndustryTrend'
      "metadata": {
        "source": "industry_report",
        "date": "2026-04-20"
      },
      "content_snippet": "프로세스화는 시간 단축률을 입증하는 핵심 지표다."
    }
    // ... 추가 노드들
  ]
}
```

#### B. 응답 본문 (`Output Payload`)

그래프 라이브러리(D3.js, React Flow 등)가 바로 사용할 수 있도록 `nodes`와 `edges` 배열로 분리하여 반환합니다. (노드는 입력된 노드들을 포함하며, Edge는 시스템이 찾아낸 연결고리를 정의합니다.)

```json
{
  "status": "success",
  "message": "지식 간 관계성 분석 완료. 총 N개의 연결 경로를 발견했습니다.",
  "nodes": [
    // 1. 입력 노드들 (Source Nodes)은 그대로 포함됨
    { "id": "uuid-1234", "label": "AI 프로세스 상품화", "type": "Concept", "position": {"x": 100, "y": 50} },
    { "id": "uuid-5678", "label": "시간 단축률 입증", "type": "KeywordCluster", "position": {"x": 300, "y": 150} }
    // ... 모든 노드 정보
  ],
  "edges": [
    {
      "id": "edge-A",                      // 연결 고리의 고유 ID (시스템 생성)
      "source": "uuid-1234",               // 시작 노드의 ID (Source Node ID)
      "target": "uuid-5678",               // 끝 노드의 ID (Target Node ID)
      "strength": 0.92,                    // 연결 강도 (0.0 ~ 1.0, 점수화)
      "relationship_type": "Causal Link", // 관계의 종류: 'Causes', 'Supports', 'RelatesTo' 등
      "reasoning_summary": "프로세스 상품화는 시간 단축률 입증을 요구하는 핵심 원인이다." // 연결 이유 요약 (가장 중요)
    }
    // ... 시스템이 찾아낸 모든 연결 고리 정보
  ]
}
```

---

### 3. 백엔드 로직 뼈대 (Pseudo Code - Python/TypeScript 스타일)

아래 코드는 실제 Graph Database(예: Neo4j, ArangoDB 등)와 연동되는 핵심 서비스 레이어의 로직을 가정합니다. 관계성 분석은 복잡한 AI 모델 호출이나 그래프 알고리즘(Graph Algorithm) 실행이 필요함을 주석으로 명시했습니다.

```python
# File: services/knowledgeService.py

from typing import List, Dict, Any
import uuid
# from graph_db_client import GraphClient # 가상의 그래프 DB 클라이언트

def analyze_and_connect(source_nodes: List[Dict[str, str]]) -> Dict[str, Any]:
    """
    입력된 파편화된 지식 노드들을 받아 관계성을 분석하고 구조화하여 반환합니다.
    """
    print("--- [SYSTEM] 지식 연결 프로세스 시작 ---")

    # 1. 노드 ID 매핑 및 초기 데이터 준비
    node_map = {node['node_id']: node for node in source_nodes}
    all_nodes: List[Dict[str, Any]] = []
    for node in source_nodes:
        # 프론트엔드가 사용할 수 있도록 위치 정보 (Position)와 더미 데이터를 추가합니다.
        all_nodes.append({
            "id": node["node_id"], 
            "label": node["content_snippet"][:30] + "...", 
            "type": node["node_type"], 
            # 실제 구현 시, AI가 이 노드 위치를 계산하거나, 클라이언트에서 초기값을 받습니다.
            "position": {"x": hash(node['node_id']) % 500 + 100, "y": len(all_nodes) * 30} 
        })

    # 2. 관계성 분석 (핵심 로직 - AI/Graph DB 연동 필요)
    print("[DEBUG] 노드 간의 잠재적 연결 경로를 그래프 알고리즘으로 탐색합니다...")
    found_edges = []
    
    # --- [🚨 이 부분이 가장 핵심적인, 복잡한 백엔드 AI 호출 영역입니다.] ---
    try:
        # 2-1. Graph DB Query 실행 (예: Cypher, Gremlin)
        # graph_client.run_query(f"MATCH (n1)-[*]->(n2) WHERE n1 IN {node_ids} AND n2 IN {node_ids}")
        
        # 2-2. Relationship Scoring 및 이유 추론 (LLM/NLP Model 호출 필요)
        # 각 잠재적 경로에 대해 '연결 강도'와 '관계성 요약'을 LLM에게 요청합니다.
        pass

    except Exception as e:
        print(f"[ERROR] 관계성 분석 중 에러 발생: {e}")
        return {"status": "error", "message": "Analysis failed."}


    # 3. 가상의 연결 고리 데이터 생성 (테스트 목적)
    found_edges = [
        {
            "id": str(uuid.uuid4()),
            "source": "uuid-1234", 
            "target": "uuid-5678", 
            "strength": 0.92,
            "relationship_type": "Causal Link", 
            "reasoning_summary": "프로세스화 경험 자체가 지적 만족감과 해방감을 유발하기 때문이다." # 핵심 문구 사용
        },
        {
            "id": str(uuid.uuid4()),
            "source": "uuid-1234", 
            "target": "another_node_id", 
            "strength": 0.65,
            "relationship_type": "Supports", 
            "reasoning_summary": "지식 연결 과정은 사용자 흐름 증명에 필수적이다."
        }
    ]

    # 4. 최종 응답 구조화 및 반환
    return {
        "status": "success",
        "message": f"{len(found_edges)}개의 강력한 지식 연결 경로를 발견했습니다.",
        "nodes": all_nodes,
        "edges": found_edges
    }

# --- 테스트 실행 (Test Run) ---
test_input = [
    {"node_id": "uuid-1234", "node_type": "Concept", "metadata": {"source": "user_input"}, "content_snippet": "AI는 단순 검색을 넘어 프로세스 자체를 상품화해야 한다."},
    {"node_id": "uuid-5678", "node_type": "KeywordCluster", "metadata": {"source": "industry_report"}, "content_snippet": "프로세스화는 시간 단축률을 입증하는 핵심 지표다."}
]

# result = analyze_and_connect(test_input)
# print("\n=== 최종 API 응답 결과 ===")
# import json
# print(json.dumps(result, indent=2, ensure_ascii=False))
```

---

### 🚀 오늘 하루의 최우선 작업 분배 및 로드맵 (2026-05-02)

[CEO 지시]와 [개인 목표/공동 목표]를 종합했을 때, 현재 가장 시급한 것은 **"기술 구현 단계 진입과 병렬적인 사용자 경험(UX) 검증"**입니다. 백엔드 API 설계는 완료되었으므로, 다음 세 가지 작업에 자원을 집중해야 합니다.

| # | 우선순위 작업 (Task) | 목표 및 필요성 | 담당 에이전트 |
| :---: | :--- | :--- | :--- |
| **1** | **[Core Logic] 지식 그래프 DB 스키마 확정 및 연결 테스트 환경 구축.** | API의 백본(Backend Backbone). Pseudo Code의 `GraphClient`가 실제로 작동할 수 있도록 데이터 모델을 정의하고, 최소한의 PoC를 위한 초기 개발 환경 설정을 완료해야 합니다. (가장 높은 기술적 난이도) | 💻 Developer |
| **2** | **[UX/Copy] 연결 과정을 설명하는 시각화 스토리보드 및 상세 와이어프레임 완성.** | 코어 기능(API)을 실제 사용자에게 어떻게 '해방감'으로 보여줄지 구체적인 인터랙션 설계가 필요합니다. (기술 스펙 $\rightarrow$ UX 플로우 매핑). | 🎨 Designer / ✍️ Writer |
| **3** | **[Strategy] 초기 PoC 데모 시나리오 및 측정 지표(KPI) 확정.** | 개발된 기능과 디자인을 통합하여, '테스트'할 명확한 사용자 흐름(User Journey)이 필요합니다. "어떤 데이터를 넣었을 때, 어떤 통찰력을 얻었는지"를 수치화하는 KPI와 시나리오가 정의되어야 합니다. | 🎯 Planner (혹은 AI 코어 통합 지점) |

**결론:** 저는 **Task 1번(DB 스키마 및 테스트 환경 구축)**에 착수하여, 이 API 로직을 실제 작동 가능한 단위로 만들겠습니다. Task 2와 3은 Designer와 Writer가 저의 기술 결과물과 상호작용할 수 있도록 병행되어야 합니다.

📊 평가: 진행중 — 핵심 백엔드 엔드포인트 구조는 정의되었으나, Pseudo Code를 실질적인 개발 환경(Graph DB)에 연결하는 작업이 필요합니다.
📝 다음 단계: 💻 Developer가 Graph Database 스키마 (예: Neo4j) 및 초기 프로젝트 설정을 위한 `requirements.txt`와 로컬 환경 세팅 가이드를 작성해야 합니다.
