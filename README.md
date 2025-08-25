# .github/workflows/update-readme.ym
name: Update README

on:
  schedule:
    - cron: '0 */6 * * *'   # هر 6 ساعت یکبار اجرا می‌شود
  workflow_dispatch:         # اجرای دستی هم فعال است

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: 📥 Checkout repo
      uses: actions/checkout@v3

    - name: 🐍 Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'

    - name: ▶️ Run script
      run: python update_readme.py

    - name: 📤 Commit & Push changes
      run: |
        git config --global user.name "github-actions[bot]"
        git config --global user.email "github-actions[bot]@users.noreply.github.com"
        git add README.md
        git commit -m "🔄 auto-update README [skip ci]" || echo "No changes to commit"
        git push
