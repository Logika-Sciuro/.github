# Instruções para o GitHub Copilot — logika-sciuro/.github

## Contexto

Este repositório contém os **padrões organizacionais** da Logika-Sciuro. Mudanças aqui se propagam para todos os repositórios da organização. Aja com cautela máxima.

## Plataforma de referência

**Scavenger Hunt** — geolocalização Zero-Knowledge:
- `scavenger-core-gateway`: Java 21+ / Quarkus / Spring WebFlux, gRPC + Kafka
- `scavenger-cv-engine`: Python 3.11+ / FastAPI / gRPC, cosine similarity (NumPy/SciPy)
- `scavenger-e2e-framework`: Java / TestNG / Maven, Mass Reader Engine + testes de contrato gRPC
- `scavenger-infra-as-code`: Terraform + Helm + Kubernetes, AWS/GCP, Zero-Trust

## Convenções de commit

Sempre seguir `.github/instructions/commit.instructions.md`:
- Formato: `<tipo>(<escopo>): <descrição em inglês, minúscula, sem ponto final>`
- Branch: `github/feature/<nome>` ou `github/fix/<nome>`
- Exemplos: `docs: update pr-reviewer agent flow`, `feat(agents): add new onboarding agent`

## Ao editar agents (`.github/agents/*.agent.md`)

- Manter frontmatter YAML: `description`, `tools`, `agents` (quando aplicável)
- Documentar: Mission, Mandatory Rules, Operational Flow, Safety Criteria, Checklist
- Nunca remover ferramentas sem avaliar impacto nos fluxos documentados
- Referências a outros arquivos do repo devem usar caminhos relativos corretos

## Ao editar instructions (`.github/instructions/*.instructions.md`)

- Manter frontmatter YAML: `name`, `description`, `applyTo`
- Exemplos devem ser concretos e específicos ao projeto Scavenger Hunt
- Não duplicar regras já definidas em outro arquivo de instrução

## Ao editar templates

- PR template (`.github/pull_request_template.md`): manter seções Resumo, Contexto, Mudanças, Como validar, Impactos, Checklist
- Issue templates: manter campos obrigatórios e labels associados (`bug_report.md`, `feature_request.md`)
- Qualquer novo campo deve ser avaliado quanto ao impacto em todos os repos que usam o template

## Ao editar workflows (`.github/workflows/`)

- Comentar propósito, gatilhos e dependências externas
- Nunca hardcodar segredos — usar `${{ secrets.NOME }}` do GitHub Secrets
- Testar em branch antes de mergear para `main`
