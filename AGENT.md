# AGENT.md — logika-sciuro/.github

## Sobre este repositório

Repositório central de padrões da organização **Logika-Sciuro**. Define templates, instruções e agentes compartilhados por todos os projetos da organização.

## Estrutura

- `.github/agents/` — definições de agentes Copilot (commit-pr-generator, pr-reviewer)
- `.github/instructions/` — arquivos de instrução para agentes (commit, PR)
- `.github/ISSUE_TEMPLATE/` — templates de issues (bug report, feature request)
- `.github/pull_request_template.md` — template padrão de PR
- `.github/workflows/` — GitHub Actions workflows da organização
- `profile/README.md` — perfil público da organização no GitHub
- `CONTRIBUTING.md` — guia base de contribuição

## Plataforma documentada

**Scavenger Hunt** — jogo de geolocalização com privacidade Zero-Knowledge:
- Cliente mobile processa GPS e imagem localmente (TensorFlow Lite + Argon2id)
- Backend nunca recebe coordenadas brutas, apenas hashes irreversíveis
- Fluxo: Mobile → Core Gateway (Java) → CV Engine (Python via gRPC) → Kafka → DynamoDB

## Regras de contribuição

- Mudanças aqui impactam **todos os repositórios** da organização — revise com cuidado
- Novos templates devem ser validados em pelo menos um repositório antes de padronizados
- Instruções de agentes devem ser claras, objetivas e testadas
- Workflows devem ter comentários explicando propósito e gatilhos

## Padrões organizacionais

### Commits
**Fonte de verdade: `.github/instructions/commit.instructions.md`**

Todos os commits devem seguir Conventional Commits conforme definido no arquivo de instrução. Verifique sempre esse arquivo para tipos, escopos, formato de descrição, body e footer.

### Branches
Prefixo obrigatório: `github/feature/<nome>` ou `github/fix/<nome>`

### Pull Requests
Template em `.github/pull_request_template.md`. Seções obrigatórias: Resumo, Contexto, Mudanças, Como validar, Impactos, Checklist.

### Agentes disponíveis
- `commit-pr-generator.agent.md` — gera commits atômicos e PRs Draft; **consulta `.github/instructions/commit.instructions.md` para validação de formato**
- `pr-reviewer.agent.md` — revisa PRs com checklist técnico; lê `AGENT.md`, `pr-instructions.md` e `commit.instructions.md` do repositório alvo

## O que NÃO fazer

- Não remover templates sem substituto aprovado pela organização
- Não alterar workflows de produção sem revisão de pelo menos um Code Owner
- Não expor segredos, credenciais ou tokens em qualquer arquivo
- Não fazer commits diretos em `main`
