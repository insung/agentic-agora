# Google Antigravity 활용 매뉴얼

Google Antigravity는 IDE(통합 개발 환경)를 "에이전트 중심(Agent-first)"으로 탈바꿈시키는 강력한 개발 플랫폼입니다. 단순한 코딩 어시스턴트를 넘어 계획, 코딩, 웹 검색 등을 수행하는 에이전트들의 "Mission Control(관제 센터)" 역할을 합니다.

## 📊 스펙 및 지원 현황 요약

| 기능 | 지원 여부 | 비고 |
| :--- | :---: | :--- |
| **Skills (스킬 시스템)** | 🟢 지원 | 재사용 가능한 지식 패키지 체계 (`SKILL.md`) |
| **Workflows (워크플로우)**| 🟢 지원 | 저장된 프롬프트/단계 시퀀스 (`/명령어`) |
| **MCP (컨텍스트 연동)** | 🟢 지원 | 외부 데이터 및 툴 연동 표준 규격 |
| **Subagent (서브에이전트)** | 🟢 지원 | 메인 에이전트가 단일 목적의 하위 에이전트 스폰(Spawn) 가능 |
| **Agent Team (에이전트 팀)** | 🟢 지원 | 에이전트 간 직접 통신 및 병렬 자율 협력 가능 |

---

## 🔗 참고 및 다운로드 (Official Links)

- **공식 사이트:** [antigravity.google](https://antigravity.google)
- **다운로드:** [antigravity.google/download](https://antigravity.google/download)
- **MCP 스토어 & 연동:** Antigravity 에디터 내 MCP Store 패널에서 직접 설정 가능

## ⚙️ 상세 설정 및 적용 경로

### 1. 스킬 (Skills)
에이전트의 지식과 능력을 확장하는 재사용 가능한 패키지입니다. 
* **글로벌 스킬 (Global):**
  - **경로:** `~/.gemini/antigravity/skills/<skill-folder>/`
* **로컬 스킬 (Local):**
  - **경로:** `<워크스페이스_루트>/.agents/skills/<skill-folder>/`
* **구성:** 반드시 `SKILL.md` 파일을 포함해야 하며, YAML 프론트매터(name, description)를 통해 정의됩니다.

### 2. 워크플로우 (Workflows)
반복적인 작업이나 특정 절차를 자동화하기 위해 저장된 프롬프트 또는 단계의 시퀀스입니다. 슬래시 명령어(예: `/workflow-name`)를 통해 즉시 트리거할 수 있습니다.
* **글로벌 워크플로우 (Global):**
  - **경로:** `~/.gemini/antigravity/global_workflows/<name>.md`
* **로컬 워크플로우 (Local):**
  - **경로:** `<워크스페이스_루트>/.agents/workflows/<name>.md`
* **특징:** 마크다운 형식으로 단계별 지침을 작성하며, `// turbo` 등의 주석을 활용할 수 있습니다.

### 3. MCP (Model Context Protocol) 연동
외부 데이터 소스나 도구를 에이전트와 연결하는 개방형 표준 프로토콜입니다.
* **글로벌 설정:** `~/.gemini/antigravity/mcp.json` 
* **로컬 설정:** `<워크스페이스_루트>/.agents/mcp.json`

#### 에이전트 아키텍처 (Agent Architecture)
* **서브에이전트 (Subagent):** 메인 에이전트가 하위 태스크를 분리하여 격리된 컨텍스트에서 수행하도록 스폰합니다.
* **서브에이전트 협력 (Subagent Coordination):** Agent Manager가 개별 서브에이전트를 스폰(Spawn)하고 조율하여 복잡한 다단계 작업을 처리합니다. MCP는 **에이전트 간 직접 통신이 아닌**, 에이전트와 외부 데이터 소스(DB, API, 클라우드 서비스 등) 간의 연결에 사용됩니다.
  - MCP 연동 예: AlloyDB, BigQuery, Spanner 등 외부 DB 또는 GitHub API 연결
  - MCP 설정 파일: `~/.gemini/antigravity/mcp_config.json` (Antigravity 에디터의 MCP Store에서도 관리 가능)
  - 참고: [antigravity.google](https://antigravity.google)

---

## 🛠 핵심 활용 방법 (Usage)

Antigravity 에이전트는 사용자와 대화하며 파일 수정, 터미널 명령어 실행(`run_command`), 웹 검색(`search_web`) 등의 툴을 자율적으로 활용합니다.
- **작업 쪼개기(STEP-BY-STEP):** 광범위한 지시보다는, 단계별로 명확한 목표(예: 트랙의 `plan.md` 항목 단위)를 부여할 때 가장 좋은 성능을 발휘합니다.
- **도구(Tools)의 자율적 사용:** 에이전트는 상황에 맞는 툴을 스스로 알아서 선택하고, 큰 작업을 앞두고는 실행 스케치를 브리핑한 뒤 기다리는 "STOP & WAIT" 프로토콜을 수행합니다.
