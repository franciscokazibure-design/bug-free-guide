# bug-free-guide
UbuntuCode

# Google App Engine generated folder
appengine-generated/
name: Publish to Zenodo

⦁	on:
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
import os
import requests

# Lê o token do ambiente (configurado em GitHub Secrets)
ZENODO_TOKEN = os.getenv("ZENODO_TOKEN")
if not ZENODO_TOKEN:
    raise Exception("Erro: ZENODO_TOKEN não está definido nos Secrets do GitHub.")

# URL da API Zenodo (sandbox para testes)
ZENODO_URL = "https://zenodo.org/api/deposit/depositions"

# Headers de autenticação
headers = {"Authorization": f"Bearer {ZENODO_TOKEN}"}

# Metadados do projeto
metadata = {
    "metadata": {
        "title": "UbuntuCode: Filosofia Transformada em Ferramenta",
        "upload_type": "publication",
        "publication_type": "article",
        "description": "UbuntuCode é um projeto científico global que transforma a filosofia Ubuntu em uma ferramenta prática de colaboração, inovação e emancipação.",
        "creators": [{"name": "Cazibure, Francisco", "affiliation": "UbuntuCode Global"}]
    }
}

# Cria o depósito (registro inicial no Zenodo)
response = requests.post(ZENODO_URL, json=metadata, headers=headers)

if response.status_code != 201:
    raise Exception(f"Falha ao criar depósito no Zenodo: {response.text}")

deposit = response.json()
deposit_id = deposit["id"]
print(f"Depósito criado com sucesso! ID: {deposit_id}")

# Exemplo: anexar README.md
files = {"file": open("README.md", "rb")}
r = requests.post(f"{ZENODO_URL}/{deposit_id}/files",
                  headers=headers,
                  data={"name": "README.md"},
                  files=files)

if r.status_code != 201:
    raise Exception(f"Erro ao enviar arquivo: {r.text}")

print("Arquivo README.md enviado com sucesso ao Zenodo!")

[ Sign in with GitHub ] [ Sign in with ORCID ] [ Sign in with Email ][ ] Zenodo would like to access your repositories
[ Authorize Zenodo ]
UbuntuCode   [ Enable ]
OutroRepo    [ Enable ]

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.123456.svg)](https://doi.org/10.5281/zenodo.123456)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.123456.svg)](https://doi.org/10.5281/zenodo.123456)
