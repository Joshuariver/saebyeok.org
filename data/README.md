# Prototype data 구조

`prototype/index.html`은 화면 렌더링 로직만 담당하고, 메뉴/본문/목록 데이터는 이 폴더의 JSON 파일에서 불러옵니다. GitHub Pages처럼 정적 파일을 제공하는 환경에서도 그대로 동작하도록 별도 빌드 과정은 두지 않았습니다.

## 파일 역할

| 파일 | 역할 |
| --- | --- |
| `index.json` | 데이터 파일 경로를 모아 둔 매니페스트. 파일이 늘어나면 먼저 여기에 등록합니다. |
| `assumptions.json` | 프로토타입 가정, 확인 필요 항목. |
| `navigation.json` | 상단/사이드 메뉴 구조. |
| `opening.json` | 아카이브를 여는 글 요약 배열, 새벽의 연혁 타임라인. |
| `years.json` | 1984~1994 연도별 개요, 사건, 관련 곡, 자료, 이미지/음원 자산. |
| `original-texts.json` | 창립자 글 및 연도별 원문 텍스트. 긴 본문은 우선 이 파일에 둡니다. |
| `discussion.json` | Google Form 링크, 게시판 링크, 토론방 mock 게시글. |
| `songs.json` | 곡 목록 및 곡 상세에 필요한 mock 메타데이터. |

## 편집 원칙

1. 메뉴 추가/삭제: `navigation.json`을 먼저 수정하고, 필요하면 해당 섹션의 데이터 JSON을 추가합니다.
2. 연도별 자료 추가: `years.json`에 구조화된 요약/사건/자산을 넣고, 긴 원문은 `original-texts.json`에 둡니다.
3. 대용량 원문/첨부: HTML에 직접 넣지 말고 JSON 또는 향후 Markdown/asset 폴더로 분리합니다.
4. 외부 링크: Notion signed URL은 만료될 수 있으므로 정식 공개 전 영구 URL 또는 별도 저장소 이전 여부를 확인합니다.
5. 새 JSON 파일 추가: `index.json`의 `files`에 경로를 등록한 뒤 `index.html`의 `loadData()`에서 읽도록 연결합니다.

## 검증

JSON 문법 확인:

```bash
python - <<'PY'
import json, pathlib
for p in pathlib.Path('prototype/data').glob('*.json'):
    json.loads(p.read_text(encoding='utf-8'))
    print('OK', p)
PY
```

로컬 확인:

```bash
cd prototype
python -m http.server 8000
```

브라우저에서 `http://localhost:8000` 접속.
