# Padrões da organização Logika-Sciuro

Este repositório centraliza arquivos padrão e templates compartilhados entre os repositórios da organização.

## Conteúdo inicial

- `profile/README.md` — perfil público da organização
- `CONTRIBUTING.md` — guia base de contribuição
- `.github/pull_request_template.md` — template padrão de pull request
- `.github/ISSUE_TEMPLATE/bug_report.md` — template de bug report
- `.github/ISSUE_TEMPLATE/feature_request.md` — template de solicitação de funcionalidade

## Objetivo

Padronizar colaboração, revisão e documentação entre todos os projetos da organização.

## Exemplo: criação de milestones

### Sessão de demonstração 1 — GitHub CLI (`gh`)

```bash
REPO="Logika-Sciuro/scavenger-infra-as-code"
gh api repos/$REPO/milestones \
  --method POST \
  -f title='Fundação & Mocking Criptográfico' \
  -f state='open'
```

### Pré-requisito (antes da próxima sessão)

Para criar milestones, a autenticação usada no `gh` ou na API precisa ter permissão de escrita em issues do repositório (ex.: escopo `repo` para repositórios privados).  
Se necessário, reautentique com:

```bash
gh auth login
gh auth status
```

### Sessão de demonstração 2 — API REST diretamente (`curl`)

```bash
OWNER="Logika-Sciuro"
REPO="scavenger-infra-as-code"
TOKEN="<SEU_TOKEN>"

curl -L -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer $TOKEN" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/$OWNER/$REPO/milestones" \
  -d '{"title":"Caminho Quente (Multiplexação gRPC)","state":"open"}'
```
