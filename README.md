# Padrões da organização Logika-Sciuro

Este repositório centraliza arquivos padrão, templates, políticas, e workflows compartilhados entre os repositórios da organização.

## 📋 Documentação

### Governança e Políticas

- **[MERGE_GATES.md](./MERGE_GATES.md)** — Critérios obrigatórios de merge (reviews, CI, documentação, commits semânticos)
- **[VERSIONING_RELEASE.md](./VERSIONING_RELEASE.md)** — Padrão de versionamento (SemVer), release workflow, e changelog
- **[CONTRIBUTING.md](./CONTRIBUTING.md)** — Guia de contribuição (règras gerais, PRs, documentação)

### Workflows e Infraestrutura de CI/CD

- **[SHARED_WORKFLOWS.md](./SHARED_WORKFLOWS.md)** — Estratégia de workflows reutilizáveis (Java, Python, Terraform, Docs)
- **[.github/workflows/README.md](./.github/workflows/README.md)** — Guia de uso dos workflows compartilhados

### Templates

- **[.github/pull_request_template.md](./.github/pull_request_template.md)** — Template padrão de pull request
- **[.github/ISSUE_TEMPLATE/bug_report.md](./.github/ISSUE_TEMPLATE/bug_report.md)** — Template de bug report
- **[.github/ISSUE_TEMPLATE/feature_request.md](./.github/ISSUE_TEMPLATE/feature_request.md)** — Template de solicitação de funcionalidade

### Organização

- **[profile/README.md](./profile/README.md)** — Perfil público da organização

---

## 🎯 Objetivo

Padronizar colaboração, revisão, CI/CD, versionamento e documentação entre todos os projetos da organização Logika-Sciuro, especialmente os repositórios da plataforma Scavenger Hunt.

---

## 🚀 Repositórios da Plataforma

Esta documentação aplica-se aos seguintes repositórios:

- `scavenger-core-gateway` (Java/Maven)
- `scavenger-cv-engine` (Python/FastAPI)
- `scavenger-e2e-framework` (Java)
- `scavenger-infra-as-code` (Terraform + Kubernetes)
- `docs` (MkDocs)
- `.github` (este repositório)

---

## 📖 Como Usar Este Repositório

### Para Contribuidores
1. Leia [CONTRIBUTING.md](./CONTRIBUTING.md) para guia geral
2. Consulte [MERGE_GATES.md](./MERGE_GATES.md) para critérios de merge
3. Verifique [VERSIONING_RELEASE.md](./VERSIONING_RELEASE.md) para standards de release
4. Use templates em `.github/ISSUE_TEMPLATE/` e `.github/pull_request_template.md`

### Para Configurar CI/CD
1. Consulte [SHARED_WORKFLOWS.md](./SHARED_WORKFLOWS.md) para entender estratégia
2. Veja [.github/workflows/README.md](./.github/workflows/README.md) para exemplos
3. Adicione `.github/workflows/build.yml` em seu repositório (referenciando workflows compartilhados)

### Para Administradores
1. Mantenha documentação atualizada conforme políticas evoluem
2. Atualize templates quando convenções mudarem
3. Comunique mudanças via PRs com discussão aberta

---

## 📌 Roadmap

### ✅ Fase 1: Pipelines Mínimos (Completo)
- Workflows reutilizáveis para Java, Python, Terraform, Docs
- Documentação de estratégia e adoção incremental

### 📍 Fase 2: Governança e Merge Gates (Em Andamento)
- Políticas de merge e review sem branch protection
- Padrão de versionamento e release (SemVer)
- **Status**: Documentação criada, adoção manual até que branch protection seja ativado

### ⏳ Fase 3: Branch Protection (Futuro)
- Ativar branch protection rules em `main`
- Reforçar automaticamente checks e approvals
- Padrão: checks obrigatórios, +1 approval

### ⏳ Fase 4: Segurança e Compliance (Futuro)
- SAST (SonarQube, CodeQL)
- Dependency scanning (Dependabot)
- SBOM generation
- Secrets scanning

---

## ❓ FAQ

**P: Quando será ativada a branch protection?**
- Fase 3 do roadmap (data TBD)
- Por enquanto, enforcement é via disciplina e code review

**P: Os workflows são obrigatórios agora?**
- Não. Fase 1 criou a base. Fase 2 será adotar em cada repo (PRs separadas)
- Adoção incremental conforme repositórios estiverem prontos

**P: Posso desviar da política de merge gates?**
- Não. Todos os critérios aplica-se para manter qualidade e consistência
- Exceções devem ser aprovadas por tech lead e documentadas

**P: Como reportar problemas com policies ou workflows?**
- Abra issue neste repositório com label `documentation` ou `ci`
- Discussão aberta para ajustes

---

## 📞 Contato

- Documentação: Este repositório
- Issues/Dúvidas: GitHub Issues (label: `question`)
- Sugestões: Pull Requests com discussão
