# KAR 업무 일정 캘린더 (Weekly Schedule App)

> 울산 KAR 기술연구소 엔지니어용 주간 브리핑 자동화 도구
> 현재 버전: **v1.0** · 단일 HTML 파일 · Vanilla JS

## 📋 이 프로젝트가 뭔가요?

매주 월요일 아침 브리핑 회의에서 쓰던 엑셀 일정표를 자동화하기 위해 만든 단일 HTML 도구입니다. 기존 워크플로우 (수작업 엑셀 업데이트 → 회의실 화면에 띄우기)를 완전히 대체합니다.

### 핵심 가치
- **단일 HTML 파일** — 빌드 도구 X, 서버 X, 그냥 브라우저에서 열면 끝
- **엑셀 직접 읽기/쓰기** — File System Access API로 기존 엑셀 파일에 in-place 저장
- **브리핑 모드** — 월요일 회의 화면 전환 (다크 테마, 큰 폰트)
- **Pretendard 한글 폰트** + **JetBrains Mono** (날짜/시간)

## 🚀 실행 방법

```bash
# Chrome 또는 Edge에서 열기만 하면 끝 (File System Access API 필요)
open 0.weekly_schedule.html          # macOS
start 0.weekly_schedule.html         # Windows
xdg-open 0.weekly_schedule.html      # Linux
```

샘플 데이터로 테스트하려면: `📂 엑셀 파일 연결` → `sample_10months.xlsx` 선택

## 📁 파일 구조

```
kar-schedule-project/
├── 0.weekly_schedule.html              # 전체 앱 (HTML + CSS + JS 단일 파일, ~87KB)
├── sample_10months.xlsx    # 샘플 데이터 (2025.8 ~ 2026.5, 49개 업무)
├── CLAUDE.md               # 이 파일 — 프로젝트 개요
├── CONTEXT.md              # 과거 작업 이력 + 설계 결정사항
└── CURRENT_STATE.md        # 현재 기능 상태 + 알려진 이슈
```

## 🛠 기술 스택

| 구성 | 사용 기술 |
|---|---|
| **UI** | Vanilla JS (No React/Vue) + CSS Grid |
| **엑셀 I/O** | SheetJS (XLSX.js) v0.18.5 CDN |
| **파일 접근** | File System Access API (Chrome/Edge 전용) |
| **영속성** | LocalStorage + IndexedDB (idb-keyval, 파일 핸들 저장) |
| **폰트** | Pretendard (한글) + JetBrains Mono (숫자) |

## 🎯 대상 사용자

- 회사: KAR (Korean Auto Research, 가상/익명화된 회사명 — 실제 자동차 R&D)
- 부서: 기술연구소 (default)
- 브라우저: **Chrome 또는 Edge 필수** (회사 PC 모두 Edge 사용 가능 확인됨)

## ⚡ 빠른 기능 개요

### 입력 뷰 (메인 화면)
- 상단 **sticky 액션바**: `새 파일로 내보내기` / `📂 엑셀 파일 연결` / `💾 저장` / `브리핑 모드 →`
- **월 네비게이션** (◀/▶, 오늘 버튼, 이번주 칩)
- **간트 스타일 캘린더** (가로축: 1~31일, 세로축: 업무 행 자동 배치)
- **이번주/다음주 섹션** (접기/펼치기, 기본 접힘)
- **이번 달 전체 업무 섹션** (접기/펼치기, 기본 접힘)
- **캘린더 뷰 토글**: 기본 / 일정 펼치기 (긴 텍스트 줄바꿈)

### 상세 모달 (캘린더 바 클릭)
- 제목 / 기간 / 세부 메모 편집
- 입력 즉시 자동 저장
- 엑셀 셀 코멘트로 메모 영속화

### 브리핑 모드 (회의용)
- 다크 테마 (`#1a1814` + gold accent `#d4a44c`)
- 지난주/이번주 토글
- 캘린더 바 & 하단 리스트 **모두 클릭해서 상세 모달**

## 📝 다음 작업 시작 가이드

1. **CONTEXT.md** 먼저 읽기 — 지금까지의 설계 결정 및 버그 히스토리
2. **CURRENT_STATE.md** 확인 — 현재 구현 상태와 알려진 제약사항
3. 파일 열기: `0.weekly_schedule.html` (단일 파일이므로 이 파일만 수정)
4. 테스트: 브라우저에서 `0.weekly_schedule.html` 열고 `sample_10months.xlsx` 연결

## 🔑 중요한 컨벤션

- **버전 배지 유지**: HTML 우측 상단 `v1.0` (사용자가 버전 증가를 요청하기 전까지 그대로)
- **한글 UX 우선**: UI 텍스트는 모두 한국어, 부서 기본값 `"기술연구소"`
- **색 팔레트**: 따뜻한 크림 배경(`#f5f2ea`) + 고동색 강조(`#8b4513`) + 금색(`#d4a44c`)
- **변경 시**: 반드시 `new Function(scriptText)`로 파싱 검증 후 저장
