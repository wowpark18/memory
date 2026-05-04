# 🎨 Designer — MVP의 핵심 가치인 '지식 연결 프로세스 시각화'를 보여주는 상세 와이어프레임(Wireframe) 프로토타입을 제작해주세요. 사용자 플로우(User Flow)가 명확하게 드러나야 하며, 노드(Node ID 포함), 관계성 엣지(Edge), 그리고 정보 단절 해소의 순간('Aha Moment')이 시각적으로 강조되는 UI 컴포넌트 디자인 브리프를 필수적으로 포함해야 합니다.

🎨 Designer: 작업 시작합니다.

[CEO 지시]에 따라, EPHA의 핵심 가치인 '지식 연결 프로세스 시각화'를 위한 와이어프레임 프로토타입과 이를 뒷받침하는 UI 컴포넌트 디자인 브리프를 작성했습니다. 이 산출물은 단순한 화면 구성을 넘어, **사용자 경험(UX) 관점에서 정보가 진화하고 지능화되는 프로세스 자체**를 상품화하는 데 초점을 맞췄습니다.

---

## 💡 EPHA: Knowledge Connection Visualization Prototype & Design Brief (v1.0)

### 📂 I. 프로젝트 개요 및 사용자 플로우 정의

*   **프로세스명:** 정보 단절 해소 루프 (Discontinuity Resolution Loop)
*   **핵심 가치:** 사용자가 "단순 지식 나열"이 아닌, "지식이 연결되는 과정(Process)"을 목격하며 '해방감'과 '지적 만족감'을 느끼게 하는 것.
*   **User Flow:**
    1.  **[Pain Point] 입력 (The Fragment):** 사용자가 무작위/파편화된 정보를 시스템에 업로드함. (Input Node 생성)
    2.  **[Processing] 연결 시도 (The Connection):** EPHA의 코어 로직이 지식 그래프를 분석하며 잠재적 관계성(Edge Candidate)을 찾음. (Graph Mapping Visualization)
    3.  **[Aha Moment] 구조화 및 증명 (The Resolution):** 시스템이 가장 강력한 연결 고리(Optimal Path)를 발견하고, 정보가 재배열되며 새로운 통찰력(Key Takeaway Node)으로 변환됨.

---

### 🎨 II. UI 컴포넌트 디자인 브리프 (필수 가이드라인)

이 브리프는 와이어프레임 전반에 걸쳐 일관되게 적용되어야 하는 시각 언어의 규칙입니다.

#### 1. 타이포그래피 및 컬러 시스템
*   **Primary Font:** Pretendard / Noto Sans (가독성 높은 산세리프 계열 유지)
    *   Headline: Bold, Inter-line spacing 1.2em
    *   Body: Regular, Inter-line spacing 1.5em
*   **Color Palette:**
    *   `--COLOR-BACKGROUND`: #0A1931 (Deep Navy/Black 계열 — 신뢰감)
    *   `--COLOR-PRIMARY-TEXT`: #E6F2FF (Off-White/Light Blue — 높은 대비)
    *   `--COLOR-CONNECTION-EDGE`: #4D7BFF (Medium Sky Blue Gradient - 연결성, 빛나는 과정)
    *   `--COLOR-NODE-FRAGMENT`: #8A9BA3 (Muted Grey - 원본, 정보 파편)
    *   `--COLOR-NODE-INSIGHT`: #6EE65E (Vibrant Green - 해결책, Aha Moment 발생 지점)

#### 2. 핵심 컴포넌트 정의
| 컴포넌트 | 기능 설명 | 디자인 규칙/시각화 요소 |
| :--- | :--- | :--- |
| **Knowledge Node** | 개별 정보 단위 (데이터 또는 개념). | 원형 베이스. `Node ID`를 상단 좌측에 미세한 폰트로 표기 (`[N-123]`). 크기와 색상으로 지식의 중요도(Fragment/Insight) 구분. |
| **Connection Edge** | Node 간의 관계성 또는 정보 흐름. | 단순 점선이 아닌, 유동적인 빛의 경로(`--COLOR-CONNECTION-EDGE` 그라디언트). 연결 강도가 높을수록 두께가 굵어짐 (Weight Visualization). |
| **Aha Moment Trigger** | 시스템이 핵심 패턴을 발견하는 순간. | 화면 전체에 걸쳐 미세한 그리드 오버레이(Grid Overlay) 효과 발생 $\rightarrow$ 배경 색상 변화를 수반하며, 관련 Node들이 일제히 `--COLOR-NODE-INSIGHT`로 빛남 (Pulse Effect). |
| **Process Log/Adapter Layer** | 시스템의 작동 원리를 설명하는 인터페이스. | 우측 사이드바에 고정된 모달 형태. "Running Process: [N-123] $\rightarrow$ [N-456]"와 같이 실시간 흐름을 텍스트로 보여주어 투명성 확보. |

---

### 💻 III. 와이어프레임 프로토타입 상세 (Screen Sequence)

#### **[Screen 1: Input State - The Fragment]**
*   **목표:** 사용자의 무질서한 데이터를 시스템에 주입하는 느낌을 극대화.
*   **레이아웃:** 메인 영역 중앙에 여러 개의 독립적이고 크기가 제각각인 `Knowledge Node`들이 떠다니는 형태 (배경 그리드 위에 불규칙하게 배치).
*   **요소:**
    *   `[N-001]`: "작년에 배운 마케팅 기법 A" (Node ID 명시, `--COLOR-NODE-FRAGMENT`)
    *   `[N-002]`: "최신 AI 트렌드와 관련한 기술적 한계점"
    *   **Action:** 하단에 배치된 'Analyze Connection' 버튼 클릭 시 다음 화면으로 전환.

#### **[Screen 2: Processing State - The Connection (Core Visualization)]**
*   **목표:** 지식 간의 관계성이 발견되고 연결되는 과정을 극적으로 보여주기.
*   **레이아웃:** Screen 1의 Node들이 중앙에 모여들고, 그 사이에 `Connection Edge`가 생성되며 움직이는 애니메이션이 핵심.
*   **요소:**
    *   **Process Log (R-Side):** "Analyzing relationship between N-001 and N-002... Candidate Edges found: 37." (시스템의 지능적인 작동을 보여주는 근거 제시)
    *   **Visualization:** Node A $\rightarrow$ Edge(약한 빛) $\rightarrow$ Node B가 생성됨. 이 과정에서 시스템이 여러 관계를 '제거'하고('Pruning'), 가장 강력한 경로만 남기는 시각적 연출 필요.

#### **[Screen 3: Output State - The Resolution (Aha Moment)]**
*   **목표:** 문제 해결의 결과물(Insight)을 확정적으로 제시하며, 사용자의 학습 욕구를 자극.
*   **레이아웃:** 화면 전체가 `--COLOR-NODE-INSIGHT`의 빛과 함께 정돈되고 구조화됨.
*   **요소:**
    1.  **Aha Moment Trigger (Visual):** 배경에 은은한 그리드 오버레이(Grid Overlay) 적용.
    2.  **Optimal Path Visualization:** Screen 2에서 연결된 Node들이 최종적으로 하나의 `Knowledge Flow Diagram`으로 재배열됨. 이 흐름이 가장 두꺼운 Edge로 강조됨.
    3.  **Key Takeaways (Structured Output):** 발견된 관계성(Process)을 바탕으로, **"새로운 프로세스 가설: 마케팅 기법 A $\rightarrow$ AI 한계점 극복 방법 B"**와 같은 구조화된 결론이 카드 형태로 제시됨.

---
📊 평가: 완료 — CEO의 지시사항(플로우, 노드 ID 포함, Aha Moment 강조)을 모두 충족하는 구체적이고 실행 가능한 디자인 브리프 및 와이어프레임 시퀀스를 제공했기 때문에 목표가 달성되었습니다.
📝 다음 단계: 이 브리프와 프로토타입을 기반으로, 실제 Figma에서 고화질의 **Component Library**를 제작하고 UI 개발팀에게 전달해야 합니다.
