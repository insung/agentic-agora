# GEMINI CLI 활용 매뉴얼

Gemini Command Line Interface (CLI)는 터미널 및 쉘 환경에 Gemini 모델을 통합하여 개발자의 로컬 워크플로우를 자동화하는 오픈소스 AI 에이전트 도구입니다.

## 📊 스펙 및 지원 현황 요약

| 기능 | 지원 여부 | 비고 |
| :--- | :---: | :--- |
| **Skills (에이전트 스킬)** | 🟢 지원 | 재사용 가능한 지식 패키지 체계 (`SKILL.md`) |
| **Sub-agents (서브에이전트)**| 🟢 지원 | (실험적) 독립된 컨텍스트를 가진 하위 에이전트 생성 |
| **Context (컨텍스트 주입)** | 🟢 지원 | `GEMINI.md` 파일을 통한 지침 제어 |
| **MCP (컨텍스트 연동)** | 🟢 지원 | `settings.json`을 통한 전역/프로젝트별 도구 연동 |
| **Multi-Agent (에이전트 팀)**| 🟢 지원 | 확장(Extensions) 및 Maestro 등을 통한 자율 협력 |

---

## 🔗 참고 및 공식 링크 (Official Links)

- **공식 사이트:** [geminicli.com](https://geminicli.com)
- **공식 문서:** [Gemini CLI Documentation](https://geminicli.com/docs)
- **GitHub 리포지토리:** [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)

---

## ⚙️ 상세 설정 및 적용 경로

### 1. 전역 설정 (`settings.json`)
CLI의 핵심 구동 및 실험적 기능을 제어합니다.
* **글로벌 설정:** `~/.gemini/settings.json`
* **서브에이전트 활성화:** 서브에이전트 기능을 사용하려면 아래 설정을 반드시 포함해야 합니다.
  ```json
  {
    "experimental": {
      "enableAgents": true
    }
  }
  ```

### 2. 에이전트 스킬 (Agent Skills)
특정 전문 지식이나 반복적인 워크플로우를 패키징한 단위입니다.
* **글로벌 스킬:** `~/.gemini/skills/`
* **로컬 스킬:** `./.gemini/skills/`
* **구조:** `SKILL.md`(지침)를 중심으로 `scripts/`, `references/` 폴더 등을 포함합니다. `gemini skills link` 명령어로 관리합니다.

### 3. 서브에이전트 구성 (Sub-agents)
메인 세션의 컨텍스트를 오염시키지 않고 특정 태스크만 전문적으로 수행하는 하위 에이전트를 정의합니다.
* **정의 방식:** YAML Frontmatter가 포함된 마크다운(`.md`) 파일을 사용합니다.
* **파일 경로:**
  - 글로벌: `~/.gemini/agents/*.md`
  - 로컬: `./.gemini/agents/*.md`
* **에이전트 정의 예시 (`security-auditor.md`):**
  ```markdown
  ---
  name: security-auditor
  description: 코드의 보안 취약점을 점검하는 전문 에이전트
  tools: [read_file, search_web]
  ---
  너는 보안 전문가이다. 주어진 코드에서 OWASP Top 10을 기준으로 취약점을 분석하고 리포트하라.
  ```

### 4. 컨텍스트 주입 (GEMINI.md)
에이전트의 페르소나 및 프로젝트 가이드를 주입합니다.
* **글로벌:** `~/.gemini/GEMINI.md` (기본 행동 강령)
* **로컬:** `./GEMINI.md` (프로젝트별 코딩 컨벤션 및 규칙)

---

## 🛠 주요 명령어 및 활용 (Usage)

- `/help` : 사용 가능한 대화형 슬래시 명령어 확인.
- `/agents` : 서브에이전트 목록 확인, 리로드 및 활성화.
- `! <command>` : 로컬 터미널 명령을 즉시 실행.
- `--prompt` (`-p`) : 한 줄의 프롬프트를 비대화식으로 실행 후 종료.
- `--yolo` (`-y`) : **YOLO(You Only Live Once) 모드.** 모든 액션(파일 수정, 명령어 실행 등)을 사용자 확인 없이 자율적으로 수행합니다. (자동화 파이프라인용)

### 멀티에이전트 및 확산 (Extensions)
`gemini extensions`를 통해 배포된 패키지는 `gemini-extension.json` 매니페스트 설정을 통해 여러 도구와 서브에이전트를 묶어서 제공합니다. **Maestro**와 같은 오케스트레이터를 연동하면 복잡한 다중 에이전트 간의 업무 분담(Frontend, Backend, QA 등)과 하이 레벨 기획(Conductor)이 가능해집니다.
