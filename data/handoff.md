# Session Handoff — 노래모임 새벽 아카이브 프로토타입

## What We Built / Fixed

### 1. founderEssay 단락 렌더링 구현
- `opening.json`의 `founderEssay` 배열(13개 단락)이 로드되지만 화면에 표시되지 않는 버그 수정
- `renderOpening()` 안에 `.essay-body` div를 추가하고 `escapeHtml()` 함수로 `<새벽>` 등 꺾쇠 문자 처리

### 2. 스마트 쿼트 버그 수정
- Edit 툴이 `"essay-body"` 내 ASCII 큰따옴표를 Unicode 곡선 따옴표(U+201D)로 자동 변환
- CSS 선택자 `.essay-body`가 DOM에서 요소를 찾지 못하는 원인이었음
- Python으로 직접 바이트 치환하여 해결 (`content.replace(u'\u201d...', '"..."')`)

### 3. 20주년 표기 수정
- `navigation.json` label: `새벽 30주년` → `새벽 20주년`
- `index.html` 본문 `<h2>` 제목 동일하게 수정
- 해당 섹션 내 오해를 줄 수 있는 편집 메모 카드 삭제

## Key Decisions
- Edit 툴 대신 Python 스크립트로 특수문자 치환 — 곡선 따옴표 재발 방지
- 영상 플레이어 연결은 실제 파일 경로/URL 확보 후 진행하기로 보류

## What's Next
- 2006 공연 `.mov` / `.mp4` 파일 위치 또는 공개 URL 확보 후 `<video>` 플레이어 추가
- 전체 파일에 스마트 쿼트가 추가로 존재하는지 일괄 검사 권장

## Gotchas
- **Edit 툴 + 따옴표**: Edit 툴로 JS 문자열 리터럴 내 `"` 를 쓰면 U+201C/U+201D로 바뀔 수 있음. HTML 속성값(`class="..."`)에 들어가면 CSS 선택자가 동작하지 않음. 특수문자 포함 라인은 Python으로 처리할 것.
- **브라우저 캐시**: 개발 중 `?v=N` 쿼리스트링으로 캐시 무효화 필요.
- **HTTP 서버 필수**: `fetch()`로 JSON 로드하므로 `file://` 직접 열기 불가. `python -m http.server 8000` 사용.
