yaml

name: Update HTML Daily
on:
  schedule:
    - cron: '0 8 * * *'  # Tous les jours à 8h
  workflow_dispatch:  # Déclenchement manuel

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Update timestamp
        run: |
          sed -i "s/Dernière mise à jour :.*/Dernière mise à jour : $(date '+%d/%m/%Y à %H:%M')/" index.html
      - name: Commit changes
        run: |
          git config user.name "GitHub Actions"
          git config user.email "actions@github.com"
          git add index.html
          git commit -m "Auto-update: $(date)" || exit 0
          git push
