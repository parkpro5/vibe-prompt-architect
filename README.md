# vibe-prompt-architect (스무고개 바이브코딩 지침 설계자)

스무고개식 질문으로 아이디어를 구체화한 뒤, **PRD → TASK → PLAN** 단계를 거쳐 하나의 통합 지침서 `VIBE_BRIEF.md`를 만들어 주는 Claude 스킬입니다. 비개발자도 코딩 에이전트(Claude Code 등)에 바로 넣어 쓸 수 있는 수준의 지침서를 만드는 것이 목표입니다.

## 주요 기능

- **스무고개 인터뷰**: 한 번에 1~3개 질문, 선택형 화면 중심, 모르면 추천 기본값(`[가정]`으로 기록)
- **결과물 유형 판단**: 업무 성격에 따라 **코드 / 스킬 / 혼합**을 판단
  - 코드·스킬 모두 가능하면 선택하게 함
  - 판단형 업무(증빙 대조·검증, 문서 작성 등)처럼 스킬이 적합하면 **스킬 제작 트랙**으로 자동 진행
- **PRD → TASK → PLAN 파이프라인**: 단계마다 승인을 받고, 요구사항 → 태스크 → 검증이 이어지도록 추적표 작성
- **최종 산출물 `VIBE_BRIEF.md` 1개**: 에이전트 운영 규칙 + PRD + TASKS + PLAN + 첫 지시문

## 설치 방법

### 방법 1. Claude 앱/웹 (claude.ai)

1. 이 저장소에서 `vibe-prompt-architect` **폴더**(안에 `SKILL.md`가 있는 폴더)를 ZIP으로 압축합니다.
   - 폴더 이름과 `SKILL.md`의 `name`(`vibe-prompt-architect`)이 같아야 합니다.
   - 저장소 전체 ZIP(Download ZIP)을 그대로 올리면 폴더 구조가 맞지 않을 수 있으니, 하위 폴더만 따로 압축하세요.
2. Claude에서 **Customize > Skills**로 이동합니다.
3. **+** 버튼 → **+ Create skill** → **Upload a skill**을 선택하고 ZIP 파일을 올립니다.
4. 목록에 나타난 스킬을 **켜기(토글 ON)** 합니다.

> 사용자 정의 스킬은 코드 실행(code execution) 기능이 켜져 있어야 하며, Free·Pro·Max·Team·Enterprise 플랜에서 제공됩니다. 메뉴 이름과 위치는 앱 버전에 따라 달라질 수 있습니다.

### 방법 2. Claude Code

스킬 폴더를 아래 위치 중 한 곳에 복사합니다.

| 위치 | 적용 범위 |
|---|---|
| `~/.claude/skills/` | 내 모든 프로젝트 |
| `<프로젝트>/.claude/skills/` | 해당 프로젝트만 |

```bash
git clone https://github.com/parkpro5/vibe-prompt-architect.git
mkdir -p ~/.claude/skills
cp -r vibe-prompt-architect/vibe-prompt-architect ~/.claude/skills/
```

Windows(PowerShell):

```powershell
git clone https://github.com/parkpro5/vibe-prompt-architect.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse .\vibe-prompt-architect\vibe-prompt-architect "$HOME\.claude\skills\"
```

설치 후 Claude Code를 다시 시작하고 `/skills`로 목록에 보이는지 확인하세요.

> claude.ai에 업로드한 스킬은 Claude 계정으로 로그인한 Claude Code와 자동 동기화됩니다(시작 시와 약 10분마다). 이 경우 Claude Code에 따로 복사하지 않아도 됩니다.

## 사용 방법

대화창에서 아래처럼 말하면 스킬이 발동됩니다.

- "스무고개로 아이디어 구체화해서 바이브코딩 지침서 만들어줘"
- "VIBE_BRIEF.md 만들어줘"
- Claude Code에서는 `/vibe-prompt-architect`

그다음은 Claude가 질문하는 대로 답하면 됩니다.

1. **인터뷰**: 목적·사용자, 결과물 유형(코드/스킬/혼합), 핵심 기능, 데이터, 제약, 성공 기준
2. **PRD → TASK → PLAN**: 단계마다 요약을 보고 승인 또는 수정
3. **통합·점검**: `VIBE_BRIEF.md` 한 파일로 완성

### 만들어진 지침서 쓰기

1. 프로젝트 폴더에 `VIBE_BRIEF.md`를 둡니다.
2. 코딩 에이전트(Claude Code 등)를 실행합니다.
3. 파일 끝의 **파트 4. 첫 지시** 문장을 붙여 넣습니다.
   - 스킬 트랙으로 만들어진 지침서는 `skill-creator` 스킬로 스킬을 만들도록 안내됩니다.

## 결과물 유형 판단 기준

| 신호 | 추천 |
|---|---|
| 내용 해석·대조·분류·요약·문서 작성 등 판단이 필요하고, 입력 형태가 가변적이며, 규칙을 직접 고치고 싶음 | 스킬 |
| 대량·정형 반복, 무인/스케줄 실행, 항상 같은 결과가 필요, 다수에게 배포할 화면·서버 필요 | 코드 |
| 판단은 필요하지만 일부 단계는 정형화 가능 | 혼합 (스킬이 판단·흐름, 스크립트가 실행) |

## 폴더 구조

```
vibe-prompt-architect/
├── README.md
└── vibe-prompt-architect/
    └── SKILL.md
```

## 문제 해결

- **업로드 오류**: ZIP 안의 폴더 이름이 `vibe-prompt-architect`인지, `SKILL.md`가 그 폴더 바로 아래 있는지 확인하세요.
- **스킬이 발동되지 않음**: Customize > Skills에서 토글이 켜져 있는지 확인하고, "스무고개로 바이브코딩 지침서 만들어줘"처럼 명확하게 요청해 보세요.
- **Claude Code에서 안 보임**: 복사 경로(`~/.claude/skills/vibe-prompt-architect/SKILL.md`)를 확인하고 Claude Code를 재시작하세요.

## 참고 문서

- [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)
- [Agent Skills 개요](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
