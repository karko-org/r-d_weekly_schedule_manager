# Claude Code 시작 프롬프트 템플릿

## 🎯 첫 프롬프트로 사용하세요

Claude Code에서 이 프로젝트를 처음 열 때 아래 프롬프트를 복사해서 붙여넣으세요:

---

```
이 프로젝트는 KAR 업무 일정 캘린더 웹앱이야.

먼저 다음 파일들을 순서대로 읽어주세요:
1. CLAUDE.md — 프로젝트 개요와 실행 방법
2. CONTEXT.md — 과거 작업 이력 + 설계 결정
3. CURRENT_STATE.md — 현재 기능 상태

그 다음 weekly_schedule.html의 구조를 파악해주세요. 단일 HTML 파일에 HTML/CSS/JS가
모두 들어있습니다 (약 87KB, ~1760 라인). 전체를 한번에 읽기보다는:
- 먼저 CSS 블록(<style>~</style>) 훑어보기
- HTML 구조 (main#input-view, main#briefing-view, .modal-overlay) 파악
- JS IIFE 안의 함수 목록 파악 (특히 render*, parseMonthlyCalendar, buildExcelBuffer)

작업 컨벤션:
- 버전 배지 'v1.5'는 사용자 요청 전까지 유지
- 변경 후 반드시 `new Function(scriptText)` 로 스크립트 파싱 검증
- UI 텍스트는 모두 한국어
- 색 팔레트 변경 금지 (CSS 변수 --bg, --accent 등 그대로 사용)

준비되면 "무엇을 작업하시겠어요?"라고 물어봐주세요.
```

---

## 💡 작업 유형별 추가 프롬프트 예시

### A. 새 기능 추가

```
[기능 이름] 기능을 추가하고 싶어.

요구사항:
- [구체적 동작 1]
- [구체적 동작 2]
- 기존 [관련 기능]과 충돌하지 않아야 함

구현 접근:
1. 어떤 파일/함수를 수정할지 먼저 알려주세요
2. CSS → HTML → JS 순서로 단계별로 보여주세요
3. 기존 패턴(예: setTwCollapsed 같은 토글 함수)을 따라주세요
```

### B. 버그 수정

```
버그가 있어. [증상 설명]

스크린샷: [파일명]

재현 방법:
1. [단계 1]
2. [단계 2]

이 버그의 원인이 뭘 것 같아? 수정 전에 원인 분석부터 보여주세요.
```

### C. 디자인 조정

```
[UI 부분] 디자인을 바꾸고 싶어.

현재:
[현재 모습 설명]

원하는 모습:
[원하는 모습 설명]

색 팔레트는 그대로 쓰되, CSS만 수정해주세요.
```

### D. 성능/구조 개선

```
[특정 부분]이 느리거나 복잡해. 리팩토링하고 싶어.

현재 코드의 문제점을 먼저 분석하고,
어떻게 개선할 수 있을지 3가지 옵션을 제시해주세요.
```

## 🛠 Claude Code가 알아야 할 중요 명령

### 빠른 검증 (변경 후)
```bash
# 스크립트 파싱 검증
node -e "
const fs = require('fs');
const html = fs.readFileSync('weekly_schedule.html', 'utf8');
const scripts = [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)];
try { new Function(scripts[scripts.length-1][1]); console.log('✓ OK'); }
catch(e) { console.log('✗', e.message); }
"
```

### 브라우저에서 로컬 서버로 테스트
```bash
# Python 3
python3 -m http.server 8000
# 그 다음 http://localhost:8000 열기

# 또는 Node
npx http-server -p 8000
```

### 샘플 파일로 테스트
1. 브라우저에서 `weekly_schedule.html` 열기
2. `📂 엑셀 파일 연결` 클릭 → `sample_10months.xlsx` 선택
3. 2025.8 ~ 2026.5 각 월 이동하며 49개 업무 확인

## 📋 커밋 메시지 컨벤션 제안 (Git 사용 시)

```
feat: 새 기능 추가 (예: "feat: 업무 색상 태그 추가")
fix: 버그 수정 (예: "fix: 브리핑 스크롤 불가 문제")
style: UI/CSS 변경 (예: "style: 모달 저장 버튼 초록색으로")
refactor: 내부 구조 개선
docs: 문서 업데이트 (CLAUDE.md 등)
```

## 🔐 파일 복구 시나리오

혹시 `weekly_schedule.html`이 손상되면:
1. `CURRENT_STATE.md`의 기능 체크리스트 확인
2. Claude Code에게 "위 체크리스트 기능들을 모두 구현한 단일 HTML 파일을 만들어줘" 요청
3. 또는 이 대화 요약본 참고하여 복원

## 📸 스크린샷 파일 네이밍 권장

피드백 시 스크린샷 첨부할 때:
- `bug-{간단설명}-{YYYYMMDD}.png` (예: `bug-overflow-20260424.png`)
- `design-{기능}-before.png` / `design-{기능}-after.png`
- 스크린샷 없이 말로만 설명하면 해결에 시간이 더 걸림 (경험상)
