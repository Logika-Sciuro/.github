---
name: Documentation Language & Style Instruction
description: This instruction defines the rules for creating and maintaining documentation across Logika-Sciuro organization repositories. All documentation must be created in Brazilian Portuguese with English translations.
applyTo: '**/*.md'
---

# Instruction for AI agent: Documentation Creation & Maintenance

Use this instruction whenever you create, update, or maintain documentation (*.md files) in any Logika-Sciuro repository.

---

## Overview

All documentation in Logika-Sciuro repositories must follow a **bilingual approach**:

1. **Primary language**: Brazilian Portuguese (português brasileiro)
2. **Secondary language**: English (international)
3. **Format**: Two files per documentation
   - `DOCUMENT_NAME.md` — Portuguese version
   - `DOCUMENT_NAME-en.md` — English version (or `DOCUMENT_NAME_EN.md`)

**Exception**: Technical documentation embedded in code (inline comments, docstrings) follows the programming language convention (typically English).

---

## File Naming Convention

### Portuguese Version (Primary)
```
README.md
CONTRIBUTING.md
MERGE_GATES.md
VERSIONING_RELEASE.md
SETUP.md
API_REFERENCE.md
TROUBLESHOOTING.md
```

### English Version (Translation)
```
README-en.md
CONTRIBUTING-en.md
MERGE_GATES-en.md
VERSIONING_RELEASE-en.md
SETUP-en.md
API_REFERENCE-en.md
TROUBLESHOOTING-en.md
```

**Alternative format** (if repo prefers `_EN` suffix):
```
README_EN.md
CONTRIBUTING_EN.md
```

**Guideline**: Use `-en.md` suffix by default for consistency. Use `_EN.md` only if the repository already established that convention.

---

## Brazilian Portuguese Standards

### Orthography & Grammar

**MUST**:
- ✅ Use correct Brazilian Portuguese orthography (Novo Acordo Ortográfico)
- ✅ Use Brazilian Portuguese conventions:
  - `ç` instead of `c` where applicable (ex: `açúcar`, `cabeça`)
  - `ã`, `õ` for nasal sounds (ex: `ação`, `informação`)
  - Accents as per ABNT standards
- ✅ Use formal language appropriate for technical documentation
- ✅ Use second person (você/vós) or imperative for instructions

**MUST NOT**:
- ❌ Use European Portuguese orthography (ex: `comecei` instead of `comecei`, `ação` not `acção`)
- ❌ Mix Portuguese with English unnecessarily (see Code References section)
- ❌ Use colloquial or informal language
- ❌ Use first person (eu/nós) excessively in imperative sections

### Examples

**Good** (Brazilian Portuguese):
```markdown
# Guia de Contribuição

## Como contribuir

1. Faça um fork do repositório
2. Crie uma branch para sua funcionalidade (`git checkout -b feature/minha-feature`)
3. Commit suas mudanças com mensagens semânticas
4. Push para a branch (`git push origin feature/minha-feature`)
5. Abra um Pull Request

## Configuração do ambiente

Você pode configurar o ambiente de desenvolvimento executando:

```bash
./scripts/setup.sh
```

Isso instalará todas as dependências necessárias.
```

**Poor** (European Portuguese or incorrect):
```markdown
# Guia de Contribuição

## Como contribuir

1. Faça uma bifurcação do repositório  ← (bifurcação = European PT)
2. Crie uma ramo para a sua funcionalidade  ← (ramo should be "branch")
3. Commit suas mudanças com mensagens semânticas
4. Envie para o ramo
5. Abra uma Solicitação de Pull  ← (too literal)

## Configuração do ambiente

O utilizador pode configurar o ambiente...  ← (utilizador = European PT)
```

---

## Code References & Technical Terms

### Inline Code References

Use backticks for code identifiers **in their original language** (typically English):

**Good**:
```markdown
Use o método `execute()` para iniciar o job.

Configure a propriedade `max.threads` no arquivo `config.yaml`.

A classe `JobProcessor` é responsável por processar filas.
```

**Poor**:
```markdown
Use o método "executar()" para iniciar o tarefa.  ← (unnecessary translation)

Configure a propriedade "máximo de threads" no arquivo "config.yaml".  ← (should be backticks)
```

### Technical Terms

Keep programming/infrastructure terms in English when they are standard terminology:

**Good**:
```markdown
Use a estratégia de cache LRU (Least Recently Used).

Configure o webhook para disparar em eventos do GitHub.

Deploy da aplicação via Docker Compose.

Implemente um circuit breaker no cliente gRPC.
```

**Poor**:
```markdown
Use a estratégia de memória auxiliar Menos Recentemente Usada.

Configure o gancho da web para disparar em eventos...

Implantação da aplicação via Compor Docker.

Implemente um interruptor de circuito no cliente...
```

---

## Document Structure

### Header & Frontmatter

Start with a clear, descriptive header in Portuguese:

```markdown
# Guia de Contribuição

Este documento descreve os padrões e políticas para contribuir com os repositórios da organização Logika-Sciuro.

---

## Seções principais
- [Regras Gerais](#regras-gerais)
- [Pull Requests](#pull-requests)
- [Documentação](#documentação)
```

### Headings & Hierarchy

- Use markdown headings (`#`, `##`, `###`)
- Use logical hierarchy (no skipping levels)
- Use descriptive titles in Portuguese

```markdown
# Título Principal

## Seção 1

### Subsection 1.1

#### Subsubsection 1.1.1
```

### Tables

Use pipes for table formatting, with Portuguese headers:

```markdown
| Tipo | Descrição | Exemplo |
|---|---|---|
| `feat` | Nova funcionalidade | `feat(api): adicionar endpoint de usuários` |
| `fix` | Correção de bug | `fix(db): corrigir leak de conexão` |
| `docs` | Atualização de documentação | `docs(readme): clarificar instruções de setup` |
```

### Lists & Numbering

Use unordered lists (`-`) for general items and ordered lists (`1.`, `2.`) for steps:

```markdown
### Itens a considerar

- Primeiro item
- Segundo item
- Terceiro item

### Passos para setup

1. Clone o repositório
2. Instale as dependências
3. Execute os testes
```

---

## English Version (Translation)

### Translation Quality

- **Use professional English** (appropriate for technical documentation)
- **Maintain parity** with Portuguese version (same structure, examples, tone)
- **Translate idioms thoughtfully** (don't translate literally word-for-word)
- **Use American English conventions** (not British) for consistency

### Translation Approach

**Paragraph-by-paragraph approach**:

Portuguese version:
```markdown
# Guia de Contribuição

## Como contribuir

1. Faça um fork do repositório
2. Crie uma branch para sua funcionalidade
```

English version:
```markdown
# Contribution Guide

## How to contribute

1. Fork the repository
2. Create a branch for your feature
```

**NOT**: Copy Portuguese structure with word-for-word translation
```markdown
# How to Contribute Guide  ← (awkward, not natural English)

## As to contribute  ← (direct translation, ungrammatical)

1. Do a fork of repository  ← (literal)
2. Create a branch to your functionality  ← (unnatural)
```

### Common Technical Translations

| Portuguese | English |
|---|---|
| repositório | repository |
| branch | branch (or "development branch" when clarity needed) |
| pull request | pull request |
| commit | commit |
| push | push |
| fork | fork |
| merge | merge |
| build | build |
| deploy | deploy |
| funcionalidade | feature |
| dependência | dependency |
| erro | error, bug (context-dependent) |
| correção | fix |
| configuração | configuration |
| propriedade | property |
| método | method |
| classe | class |
| pipeline | pipeline |

---

## Cross-References & Links

### Internal Links

Use relative links and maintain same structure for both versions:

**Portuguese**:
```markdown
Ver [MERGE_GATES.md](./MERGE_GATES.md) para critérios de merge.

Consulte também [VERSIONING_RELEASE.md](./VERSIONING_RELEASE.md).
```

**English version**:
```markdown
See [MERGE_GATES-en.md](./MERGE_GATES-en.md) for merge criteria.

Also refer to [VERSIONING_RELEASE-en.md](./VERSIONING_RELEASE-en.md).
```

### External References

Keep URLs as-is; only translate surrounding text:

```markdown
Para mais informações, visite [Semantic Versioning](https://semver.org/).

Leia o [guia oficial](https://docs.github.com/en/actions).
```

---

## Examples: Before & After

### Example 1: README

**Poor Portuguese**:
```markdown
# Projeto Gateway

Este é o projeto central da plataforma. Usa Java 21 e Maven. Deploy com Docker.

Para instalar: execute `mvn clean install`. Deploy via `docker build`.

Ver documentação em `docs/`.
```

**Good Portuguese**:
```markdown
# Scavenger Core Gateway

Este repositório contém o gateway central da plataforma Scavenger Hunt.

## Tecnologias

- Java 21 (LTS)
- Apache Maven 3.9+
- Docker & Docker Compose

## Configuração do Ambiente

### Pré-requisitos

- JDK 21+ instalado
- Maven 3.9+ instalado

### Instalação

1. Clone o repositório
2. Instale as dependências:

```bash
mvn clean install
```

3. Execute os testes:

```bash
mvn test
```

### Build & Deploy

Para fazer build da imagem Docker:

```bash
docker build -t scavenger-core-gateway:latest .
```

Para executar localmente:

```bash
docker-compose up
```

Consulte [docs/](./docs/) para documentação completa.
```

**Good English**:
```markdown
# Scavenger Core Gateway

This repository contains the central gateway for the Scavenger Hunt platform.

## Technologies

- Java 21 (LTS)
- Apache Maven 3.9+
- Docker & Docker Compose

## Environment Setup

### Prerequisites

- JDK 21+ installed
- Maven 3.9+ installed

### Installation

1. Clone the repository
2. Install dependencies:

```bash
mvn clean install
```

3. Run tests:

```bash
mvn test
```

### Build & Deploy

To build the Docker image:

```bash
docker build -t scavenger-core-gateway:latest .
```

To run locally:

```bash
docker-compose up
```

See [docs/](./docs/) for full documentation.
```

---

## Maintenance & Updates

### Keeping Versions in Sync

When updating documentation:

1. **Always update both versions** (Portuguese and English)
2. **Update on same commit** (both files in one PR)
3. **Include both in commit message**:
   ```
   docs(readme): update installation instructions
   
   - Updated CONTRIBUTING.md and CONTRIBUTING-en.md
   - Clarified Maven version requirement
   - Added troubleshooting section
   ```

### Version Control

Both versions are equally important:

```bash
# Good: Update both together
git add CONTRIBUTING.md CONTRIBUTING-en.md
git commit -m "docs(contributing): update workflow guidelines"

# Avoid: Updating only one version
git add CONTRIBUTING.md
git commit -m "docs(contributing): update workflow"
# ❌ Missing CONTRIBUTING-en.md update
```

---

## Review Checklist for Documentation

When reviewing documentation PRs, verify:

### Portuguese Version
- [ ] Correct Brazilian Portuguese orthography
- [ ] No unnecessary English words (except technical terms)
- [ ] Clear, formal language appropriate for documentation
- [ ] Consistent terminology throughout document
- [ ] All headings descriptive and hierarchical
- [ ] All code references in backticks

### English Version
- [ ] Professional, clear English
- [ ] Parity with Portuguese version (same structure, content)
- [ ] Consistent technical terminology
- [ ] No direct word-for-word translation artifacts
- [ ] Proper grammar and style

### Both Versions
- [ ] Updated together in same commit
- [ ] Internal links point to correct files (-en.md or .md)
- [ ] Tables, lists, and code blocks formatted correctly
- [ ] No broken external links
- [ ] Tone and level consistent

---

## When to Apply This Instruction

This instruction applies to:

- ✅ README.md (and README-en.md)
- ✅ CONTRIBUTING.md
- ✅ MERGE_GATES.md
- ✅ VERSIONING_RELEASE.md
- ✅ TROUBLESHOOTING.md
- ✅ API documentation (*.md files)
- ✅ Architecture Decision Records (ADRs)
- ✅ Guides, tutorials, how-tos

This instruction does NOT apply to:

- ❌ Code comments (follow programming language conventions)
- ❌ Docstrings (follow language documentation standards)
- ❌ Configuration files (keep technical names as-is)
- ❌ Git commit messages (follow conventional commits in English)

---

## References

- [Novo Acordo Ortográfico](https://www.academia.brasileira.de.letras.org.br/a-academia/history) — Brazilian Portuguese orthography standard
- [ABNT NBR 6023](https://www.abnt.org.br/) — Brazilian standards organization
- [Chicago Manual of Style](https://www.chicagomanualofstyle.org/) — English documentation style reference
- [GitHub Markdown Guide](https://guides.github.com/features/mastering-markdown/)

---

## FAQ

**Q: Why bilingual documentation?**
- Team has both Portuguese and English speakers
- Increases accessibility for international collaborators
- Maintains language inclusivity

**Q: Can I use machine translation (ChatGPT, Google Translate)?**
- For initial draft: Yes, but must be reviewed by native speaker
- For final version: No, always use human review
- Always verify technical accuracy, not just grammatical correctness

**Q: What if a document is only for internal Portuguese team?**
- Still provide English version for future accessibility
- English version can be marked as optional/informational

**Q: Should code examples be translated?**
- No. Keep code, variable names, function names, etc. in English
- Translate only surrounding explanation

**Q: How long should I spend on translation?**
- Portuguese → English: ~30-50% of original writing time
- Focus on clarity and technical accuracy, not literalness

---

## Contact

For questions about documentation style and standards:
- Review: [CONTRIBUTING.md](./CONTRIBUTING.md) for contribution guidelines
- GitHub Issues: Label with `documentation`
