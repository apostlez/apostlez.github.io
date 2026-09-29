# apostlez.github.io

[apostlez.github.io](https://apostlez.github.io) — 개인 게임 포털 사이트.

---

## 게임 목록 / Games

| 게임 | URL | 소스 |
|------|-----|------|
| 달려라 두부 🐹 | [/anna_games/run_dubu_run/](https://apostlez.github.io/anna_games/run_dubu_run/) | [anna_games](https://github.com/apostlez/anna_games) |
| 우주 슈팅 🚀 | [/anna_games/gun_shoot/](https://apostlez.github.io/anna_games/gun_shoot/) | [anna_games](https://github.com/apostlez/anna_games) |
| Word Wizard 🪄 | [/anna_games/word_wizard/](https://apostlez.github.io/anna_games/word_wizard/) | [anna_games/word_wizard](https://github.com/apostlez/anna_games/tree/main/word_wizard) |

---

## 저장소 구조

`anna_games` 저장소를 **git submodule** 로 포함합니다.  
`apostlez.github.io` 저장소를 클론할 때는 반드시 서브모듈도 함께 초기화하세요.

```bash
git clone --recurse-submodules https://github.com/apostlez/apostlez.github.io.git

# 이미 클론한 경우
git submodule update --init --recursive
```

---

## 배포 방식

- **플랫폼**: GitHub Pages
- **게임 소스**: `anna_games` 서브모듈 (`./anna_games/`)
- **자동 배포**: `.github/workflows/deploy-pages.yml`이 Word Wizard를 production build한 뒤 Pages artifact로 배포

GitHub 저장소 Settings → Pages → Build and deployment → Source에서 **GitHub Actions**를 선택해야 합니다.

### Word Wizard 로컬 실행

```bash
cd anna_games/word_wizard
npm install
npm run dev
```

production 검증:

```bash
npm test
npm run validate:words
npm run build
```

### anna_games 업데이트 반영

`anna_games` 저장소에 새 커밋이 생기면 아래 명령으로 서브모듈 포인터를 갱신합니다.

```bash
git submodule update --remote anna_games
git add anna_games
git commit -m "chore: update anna_games submodule"
git push origin main
```

`anna_games`의 Word Wizard 소스가 변경되면 workflow가 `npm run build`를 실행하고, 결과를 `/anna_games/word_wizard/` 경로에 게시합니다.

---

## 주의사항

- 게임은 ES Modules(`type="module"`)을 사용하므로 **`file://` 직접 열기는 동작하지 않습니다**.  
  로컬 테스트 시 반드시 로컬 서버를 사용하세요.

  ```bash
  # Python
  python -m http.server 8080

  # Node.js
  npx serve .
  ```

- `apostlez.github.io` 저장소는 **HTTPS submodule URL** 을 사용해야 GitHub Pages가 서브모듈을 정상 빌드합니다.  
  SSH URL(`git@github.com:...`)은 Pages 환경에서 접근 불가합니다.