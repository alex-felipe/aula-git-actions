# aula-git-actions

# Fluxo de trabalho simples para implantar conteúdo estático no GitHub Pages
name: Implantar conteúdo estático no Pages

on:
  push:
    branches: ["main"]

  # Permite que você execute este fluxo de trabalho manualmente a partir da aba Ações
  workflow_dispatch:

# Define as permissões do GITHUB_TOKEN para permitir a implantação no GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Permite apenas uma implantação concorrente, ignorando execuções enfileiradas entre a execução em andamento e a mais recente enfileirada.
# No entanto, NÃO cancele execuções em andamento, pois queremos permitir que essas implantações de produção sejam concluídas.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Job único de implantação, já que estamos apenas implantando
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Configurar Pages
        uses: actions/configure-pages@v5
      - name: Fazer upload do artefato
        uses: actions/upload-pages-artifact@v3
        with:
          # Fazer upload de todo o repositório
          path: '.'
      - name: Implantar no GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
