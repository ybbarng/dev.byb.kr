# dev.byb.kr

ybbarng이 이것저것 만들어 굴려보는 **개인 실험 허브**. 진지한 것도, 장난 같은 것도 한곳에 모아둡니다.

🔗 **https://dev.byb.kr**

## 구조

```
dev.byb.kr/
├── index.html          # 허브 — 프로젝트 카드를 태그별로 보여줌(정적 HTML 한 장)
├── CNAME               # dev.byb.kr (GitHub Pages 커스텀 도메인)
├── worldcup-2026/      # 내부 프로젝트(폴더 = 경로). dev.byb.kr/worldcup-2026
│   └── index.html
└── <다음 프로젝트>/     # 내부면 폴더로 추가, 외부면 index.html의 배열에만 추가
```

허브는 빌드 도구·프레임워크 없이 **정적 HTML 한 장**입니다. 프로젝트 목록은 `index.html` 안의 `PROJECTS` 배열이 전부입니다.

## 프로젝트

| 프로젝트 | 태그 | 시점 |
|---|---|---|
| [SET](https://set.byb.kr/) — 보드게임 SET | 🕹️ Game | 2016 |
| [GORADOS](https://gorados.byb.kr/) — 포켓몬GO 지도(목업) | 🎮 Toy | 2017 |
| [tweet-manager](https://github.com/ybbarng/tweet-manager) — 트윗 관리 앱 | 🧪 Lab | 2026 |
| [Français Très Facile](https://ftf.byb.kr/) — RFI 프랑스어 학습 | 🧪 Lab | 2026 |
| [Claude Buddy Showcase](https://gh.byb.kr/claude-buddy-showcase/) | 🎮 Toy | 2026 |
| [worldcup-2026](./worldcup-2026/) — 데이터로 본 2026 월드컵 | 🧪 Lab | 2026 |

## 새 프로젝트 추가하기

`index.html`의 `PROJECTS` 배열에 한 항목만 넣으면 됩니다.

```js
{
  href:  './slug/',          // 내부: 폴더 경로 / 외부: 'https://...' / 코드만: GitHub URL
  emoji: '🎨',
  tag:   'toy',              // 'lab' | 'toy' | 'game'
  title: '프로젝트 이름',
  desc:  '한 줄 설명(성격은 여기에)',
  meta:  '2026년 6월',        // 만든 시점만 (성격은 desc에)
},
```

- **내부 프로젝트**면 `slug/` 폴더에 `index.html`을 추가합니다 → `dev.byb.kr/slug`
- **외부/앱**이면 배열 항목만 추가하면 됩니다(링크는 자동으로 새 탭). GitHub URL이면 카드에 "GitHub ↗"로 표시됩니다.
- 카드는 **만든 시점 순(오래된 것 위)**으로 둡니다.

## 태그

| 태그 | 뜻 |
|---|---|
| 🧪 **Lab** | 본격 실험·도구·학습 (데이터 시각화, 신기술 데모 등) |
| 🎮 **Toy** | 가벼운 창작·서비스·데모 |
| 🕹️ **Game** | 게임 |

## 로컬 확인 · 배포

```bash
open index.html          # 브라우저로 바로 확인 (빌드 없음)
```

- **배포**: GitHub Pages — `main` 브랜치 `/(root)`. `CNAME` 파일이 있어 도메인은 자동 인식됩니다.
- byb.kr DNS에서 `dev` 서브도메인을 `ybbarng.github.io`로 연결.

---
made by [@ybbarng](https://github.com/ybbarng) · [blog](https://gh.byb.kr)
