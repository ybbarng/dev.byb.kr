# dev.byb.kr — 작업 가이드 (AI/사람 공용)

이 저장소는 ybbarng의 **개인 실험 허브**다. `index.html`(정적 HTML 한 장)이 프로젝트 카드를 태그별로 보여주고, 각 프로젝트는 **내부 폴더** 하나이거나 **외부 링크**다. 빌드 도구·프레임워크는 쓰지 않는다.

응답·커밋·문서는 **한국어**로 쓴다.

## 프로젝트 추가/수정

프로젝트 목록은 오직 `index.html`의 `const PROJECTS = [...]` 배열이다. 항목 하나가 카드 하나다.

```js
{
  href:  './slug/',          // 내부: 폴더 경로 / 외부 사이트: 'https://...' / 코드만: GitHub URL
  emoji: '🎨',
  tag:   'toy',              // 'lab' | 'toy' | 'game'
  title: '프로젝트 이름',
  desc:  '한 줄 설명 — 무엇을 하는지, 성격은 여기에',
  meta:  '2026년 6월',        // 만든 시점만. 성격·키워드를 넣지 않는다(그건 desc·tag가 맡음)
},
```

- **내부 프로젝트**면 `slug/` 폴더 + `slug/index.html`을 함께 추가한다 → `dev.byb.kr/slug`.
- **외부 사이트/앱**이면 배열 항목만 추가한다. 렌더가 알아서 새 탭(`target=_blank`)으로 열고, GitHub URL이면 카드 링크를 "GitHub ↗", 그 외 외부는 "바로가기 ↗", 내부는 "열기 →"로 표시한다.

## 정렬·태그·표기 규칙

- **정렬**: 카드는 *만든 시점 순*(오래된 것이 위)으로 둔다. 시점이 헷갈리면 저장소 `createdAt`이나 git history로 확인하고, 애매하면 사용자에게 물어본다.
- **태그**: `lab` 🧪 = 본격 실험·도구·학습 · `toy` 🎮 = 가벼운 창작·서비스·데모 · `game` 🕹️ = 게임. 새 태그가 필요하면 CSS(`--색`, `.badge.태그`)·필터 버튼·`TAGLABEL`을 함께 추가한다.
- **meta**: 시점만(예 `2016년`, `2026년 6월`). 같은 해가 여럿이면 월까지 적어 순서를 분명히 한다.

## 검증·커밋·배포

- 카드/로직을 고치면 스크립트 구문을 확인한다:
  ```bash
  python3 -c "import re;h=open('index.html',encoding='utf-8').read();s=re.findall(r'<script>(.*?)</script>',h,re.S)[0];open('/tmp/hub.js','w').write(s)" && node --check /tmp/hub.js
  ```
- 커밋 메시지는 한국어. 요청하지 않은 변경은 끼워 넣지 않는다.
- **배포**: GitHub Pages(`main` 브랜치 `/(root)`). `CNAME`=`dev.byb.kr`이 있어 도메인은 자동 인식된다. push하면 몇 분 뒤 반영.

## 이력 메모

- `worldcup-2026`은 2026 월드컵 예측·회고 페이지의 **최종 정적 스냅샷**이다. 대회 기간에는 별도 작업 저장소(`~/worldcup`)에서 매일 시뮬레이션을 돌려 발행했고, 대회 종료 후 결과물만 이 허브로 옮겼다. 발행 파이프라인·시뮬레이션 코드는 여기에 없다.
- `byb.kr/korea-wc2026.html`은 nginx에서 `dev.byb.kr/worldcup-2026/`으로 301 리다이렉트된다.
