# 💫 About Me:
I'm passionate about building modern web applications,<br>beautiful interfaces,<br>and AI-powered automation.<br><br>Currently focusing on:<br><br>Frontend Development<br>UI/UX<br>AI Automation<br>React<br>Next.js<br>Open Source


# 💻 Tech Stack:
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![Vercel](https://img.shields.io/badge/vercel-%23000000.svg?style=for-the-badge&logo=vercel&logoColor=white) ![WordPress](https://img.shields.io/badge/WordPress-%23117AC9.svg?style=for-the-badge&logo=WordPress&logoColor=white) ![Next JS](https://img.shields.io/badge/Next-black?style=for-the-badge&logo=next.js&logoColor=white) ![Angular](https://img.shields.io/badge/angular-%23DD0031.svg?style=for-the-badge&logo=angular&logoColor=white) ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![MySQL](https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white) ![Canva](https://img.shields.io/badge/Canva-%2300C4CC.svg?style=for-the-badge&logo=Canva&logoColor=white) ![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white) ![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white) ![Bitwarden](https://img.shields.io/badge/bitwarden-%23175DDC.svg?style=for-the-badge&logo=bitwarden&logoColor=white) ![Cisco](https://img.shields.io/badge/cisco-%23049fd9.svg?style=for-the-badge&logo=cisco&logoColor=black) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-%23000000.svg?style=for-the-badge&logo=notion&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white) ![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)
# 📊 GitHub Stats:
![](https://github-readme-stats.shion.dev/api?username=recap-publish&theme=rose_pine&hide_border=false&include_all_commits=true&count_private=true)<br/>
![](https://streak-stats.demolab.com/?user=recap-publish&theme=rose_pine&hide_border=false)<br/>
![](https://github-readme-stats.shion.dev/api/top-langs/?username=recap-publish&theme=rose_pine&hide_border=false&include_all_commits=true&count_private=true&layout=compact)

name: main

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1
      - uses: oven-sh/setup-bun@735343b667d3e6f658f44d0eca948eb6282f2b76 # v2.0.2
        with:
          bun-version: 1.3.12

      - run: bun install --frozen-lockfile

      - run: npm run type
      - run: npm run lint
      - run: bun test
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  test-lib:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1
      - uses: oven-sh/setup-bun@735343b667d3e6f658f44d0eca948eb6282f2b76 # v2.0.2
        with:
          bun-version: 1.3.12

      - run: bun install --frozen-lockfile

      - run: bun build:lib

      - run: mv ./packages/generate-snake-animation/dist ${{ runner.temp }}/snk

      - name: run via npx
        run: |
          npx --yes file:${{ runner.temp }}/snk --github_user=platane --output=snake.svg
          test -f snake.svg
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - run: test -f snake.svg

      - name: install dependencies canvas, gifslice.. in the isolated env parent dir
        run: bun add --cwd ${{ runner.temp }} canvas@3.2.0 gif-encoder-2@1.0.5 gifsicle@5.3.0

      - run: bun ${{ runner.temp }}/snk/cli.js --github_user=platane --output=snake.gif
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - run: test -f snake.gif

  test-action:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1

      - name: update action.yml to use image from local Dockerfile
        run: |
          sed -i "s/image: .*/image: Dockerfile/" action.yml

      - name: generate-snake-game-from-github-contribution-grid
        id: generate-snake
        uses: ./
        with:
          github_user_name: platane
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
            dist/github-contribution-grid-snake-ocean.svg?color_snake=orange&color_dots=#dadcdf94,#8dbdff,#64a1f4,#4b91f1,#3c7dd9
            dist/github-contribution-grid-snake-grey.svg?color_snake=111&color_dots=#eee,#aaa,#888,#666,#444
            dist/github-contribution-grid-snake.gif
            dist/github-contribution-grid-snake-dark.gif?palette=github-dark
            dist/github-contribution-grid-snake-ocean.gif?color_snake=orange&color_dots=#dadcdf94,#8dbdff,#64a1f4,#4b91f1,#3c7dd9&color_background=#f7f8fa

      - name: ensure the generated file exists
        run: |
          ls dist
          test -f dist/github-contribution-grid-snake.svg
          test -f dist/github-contribution-grid-snake-dark.svg
          test -f dist/github-contribution-grid-snake-ocean.svg
          test -f dist/github-contribution-grid-snake-grey.svg
          test -f dist/github-contribution-grid-snake.gif
          test -f dist/github-contribution-grid-snake-dark.gif
          test -f dist/github-contribution-grid-snake-ocean.gif

      - uses: crazy-max/ghaction-github-pages@df5cc2bfa78282ded844b354faee141f06b41865 # v4.2.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  test-action-svg-only:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1
      - uses: oven-sh/setup-bun@735343b667d3e6f658f44d0eca948eb6282f2b76 # v2.0.2
        with:
          bun-version: 1.3.12

      - run: bun install --frozen-lockfile

      - name: build svg-only action
        run: |
          npm run build:action
          rm -r svg-only/dist
          mv packages/action/dist svg-only/dist

      - name: generate-snake-game-from-github-contribution-grid
        id: generate-snake
        uses: ./svg-only
        with:
          github_user_name: platane
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
            dist/github-contribution-grid-snake-ocean.svg?color_snake=orange&color_dots=#dadcdf94,#8dbdff,#64a1f4,#4b91f1,#3c7dd9
            dist/github-contribution-grid-snake-grey.svg?color_snake=111&color_dots=#eee,#aaa,#888,#666,#444

      - name: ensure the generated file exists
        run: |
          ls dist
          test -f dist/github-contribution-grid-snake.svg
          test -f dist/github-contribution-grid-snake-dark.svg
          test -f dist/github-contribution-grid-snake-ocean.svg
          test -f dist/github-contribution-grid-snake-grey.svg

      - uses: crazy-max/ghaction-github-pages@df5cc2bfa78282ded844b354faee141f06b41865 # v4.2.0
        with:
          target_branch: output-svg-only
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  deploy-ghpages:
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1
      - uses: oven-sh/setup-bun@735343b667d3e6f658f44d0eca948eb6282f2b76 # v2.0.2
        with:
          bun-version: 1.3.12

      - run: bun install --frozen-lockfile

      - run: npm run build:demo
        env:
          GITHUB_USER_CONTRIBUTION_API_ENDPOINT: https://github-user-contribution.platane.workers.dev/github-user-contribution/

      - uses: actions/upload-pages-artifact@7b1f4a764d45c48632c6b24a0339c27f5614fb0b # v4.0.0
        with:
          path: packages/demo/dist

      - uses: actions/deploy-pages@d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e # v4.0.5
        if: success() && github.ref == 'refs/heads/main'

      - run: bun wrangler deploy
        if: success() && github.ref == 'refs/heads/main'
        working-directory: packages/github-user-contribution-service
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}

## 🏆 GitHub Trophies
![](https://github-profile-trophy.vercel.app/?username=recap-publish&theme=transparent&no-frame=false&no-bg=false&margin-w=4)

### 🔝 Top Contributed Repo
![](https://github-contributor-stats.vercel.app/api?username=recap-publish&limit=5&theme=transparent&combine_all_yearly_contributions=true)

---
[![](https://komarev.com/ghpvc/?username=recap-publish&icon=0&color=5)](https://visitcount.itsvg.in)

<!-- Proudly created with GPRM ( https://gprm.itsvg.in ) -->
