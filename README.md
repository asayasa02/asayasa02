name: Generate Snake

on:
  schedule:
    - cron: "0 */12 * * *"   # otomatis tiap 12 jam
  workflow_dispatch:          # bisa dijalankan manual
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Generate snake SVG
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg?palette=github-light&color_snake=F75590&color_dots=FFF0F5,FFB6C1,FF8FB1,F75590,C2185B
            dist/github-snake-dark.svg?palette=github-dark&color_snake=F75590

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
