# bug-free-guide
UbuntuCode

# Google App Engine generated folder
appengine-generated/
name: Publish to Zenodo

# .github/workflows/publish-to-zenodo.yml
name: Publish to Zenodo

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install requests

      - name: Publish to Zenodo
        env:
          ZENODO_TOKEN: ${{ secrets.ZENODO_TOKEN }}
        run: |
          echo "Publicando release no Zenodo..."
          python scripts/publish_to_zenodo.py


