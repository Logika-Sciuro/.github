---
name: Semantic Commit Message Instruction
description: This instruction defines the rules and format for generating semantic commit messages in this repository, following the conventional commits standard.
applyTo: '**'
---


# Instruction for AI agent: generate commit message

Use this instruction whenever you need to create commit messages in this repository.

## Objective

Generate a semantic commit message, clear, short, and compliant with the **conventional commits** standard

## Message Format
```
<type>(<scope>): <description>

<optional body>

<optional footer>
```

## 1. Commit Type

Allowed commit types:

| Type         | Description                                                                                                                                                                                                                                               | Example                                                            |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| **feat**     | Includes a new feature (relates to **MINOR** in semantic versioning).                                                                                                                                                                                    | `feat(execução): adicionar suporte a execução paralela`            |
| **fix**      | Solves a problem (relates to **PATCH** in semantic versioning).                                                                                                                                                                                          | `fix(driver): corrigir leak de ThreadLocal em cleanup`             |
| **docs**     | Documentation changes, such as in your repository's Readme. (Does not include code changes).                                                                                                                      | `docs: adicionar guia de setup de desenvolvimento`                 |
| **style**    | Changes related to code formatting, semicolons, trailing spaces, lint... (Does not include code changes).                                                                                                           | `style: remover linhas em branco excessivas`                       |
| **refactor** | Changes due to refactoring that do not alter functionality, such as changing how a part of the screen is processed but keeping the same functionality, or performance improvements due to code review.                | `refactor(contexto): simplificar lógica de merge de parâmetros`    |
| **perf**     | Code changes related to performance.                                                                                                                                                                               | `perf(driver): otimizar inicialização com cache`                   |
| **test**     | Changes in tests, whether creating, changing, or deleting unit tests. (Does not include code changes)                                                                                                              | `test(lifecycle): adicionar testes de sincronização`               |
| **chore**    | Updates to build tasks, admin settings, packages... such as adding a package to gitignore. (Does not include code changes)                                                                                          | `chore: atualizar JUnit para 4.13.2`                               |
| **ci**       | Changes related to continuous integration.                                                                                                                                                                         | `ci: adicionar validação de código no GitHub Actions`              |
| **revert**   | Reverting previous commits                                                                                                                                                                                         | `revert: reverter feat(driver) que causou regressão`               |
| **build**    | Modifications in build files and dependencies.                                                                                                                                                                     | `build: adicionar plugin de verificação de dependências`           |
| **raw**      | Changes related to configuration files, data, features, parameters.                                                                                                                                                | `raw(config): adicionar nova propriedade de configuração de debug` |
| **cleanup**  | Remove commented code, unnecessary snippets, or any other form of code cleanup, aiming to improve readability and maintainability                                                                                 | `cleanup: remover código comentado e imports não utilizados`       |
| **remove**   | Deletion of **obsolete** or **unused** files, directories, or features, reducing project size and complexity and keeping it more organized                                                                       | `remove: excluir classe de utilitário obsoleta`                    |

## 2. Scope

Suggested scopes:

### Functional Scopes
<!-- Map repository folders to conceptual layers to define scopes. Adjust these examples based on your project's structure. -->
- `logger` — structured logging
- `config` — system properties, general configuration
- `infra` — general infrastructure (not classified above)
- `shared` — utilities, exceptions, transversal constants

### Maintenance Scopes

- `dependencies` — dependency updates
- `ci` — pipeline, GitHub Actions
- `build` — Dependency management and build configuration (ex: Maven, Gradle, NPM files)

### Example with Scope
```
feat(logger): add support for structured logging with Log4j2
fix(config): correct default value for `a.b.cd` property
docs(ci): update GitHub Actions workflow documentation

```

## 3. Description

The description must:

- **MUST** be in english, with the first letter after the prefix in lowercase
- **MUST** be concise and objective (maximum 50 characters, ideally)
- **MUST** use the verb in the infinitive (implement, fix, add, etc.)
- **MUST NOT** have a period at the end of the line

### Good examples

```
feat(config): implement support for logging with Log4j2
fix(config): correct null values in parameters
docs(config): document debug properties
test(config): add tests for log4j2 configuration
```

### Incorrect examples

```
feat(config): implement support for logging with Log4j2.       --> uppercase letter, period
fix(config): correcting null values in parameters              --> gerund, period
test(config): add tests                                        --> generic description

```

## 4. Body (Optional, but recommended)

The body should contain additional contextual information, such as:

- **What** was done
- **Why** it was needed
- **How** it was implemented when relevant

### Rules

- Markdown MUST BE used for better readability
- Classes and methods MUST BE referenced with full name and code markdown (e.g.: `GerenciadorDriverMobile.inicializar()`)
- Configuration properties AND variables MUST BE referenced with code markdown (e.g.: `debug.fake.driver.mobile.fallback`, `BASE_URL`)
- Files MUST BE referenced with italic markdown (e.g.: _README.md_)

### Body Example

```
feat(image-processing): add support for image resizing with OpenCV

This commit adds a new `ImageResizer` class that uses OpenCV to resize images while maintaining aspect ratio. The main motivation was to optimize image loading times in the application by allowing dynamic resizing based on device capabilities.
The implementation includes:
- New `ImageResizer` class with `resize()` method
- Integration with existing image processing pipeline in `ImageProcessor`
- Unit tests for `ImageResizer` in `ImageResizerTest`
```

## 5. Footer (Optional)

Use the footer to:

- Indicate a **breaking change**
- Point to **code that needs review**

### Format

```
<keyword>: <description or reference>
```

### Keywords

| Keyword           | Meaning                                                                                                              | Example                                             |
|-------------------|----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| `BREAKING CHANGE` | introduces a change that breaks API compatibility (relates to MAJOR in semantic versioning)                         | `BREAKING CHANGE: RemovREST API deprecated em v1.x` |
| `depends-on`      | Indicates dependency on another commit or PR                                                                         | `depends-on: abc123def456`                          |
| `related`         | Indicates relation to another commit, PR or issue                                                                    | `related: abc123def456`                             |
| `supersedes`      | Indicates this commit supersedes another previous commit                                                             | `supersedes: abc123def456`                          |
| `cherry-picks`    | Indicates this commit is a copy of another commit from a different branch                                            | `cherry-picks: abc123def456`                        |
| `backport`        | Indicates this commit is a copy of another commit for a maintenance branch                                           | `backport: abc123def456`                            |
| `forward-port`    | Indicates this commit is a copy of another commit for a development branch                                           | `forward-port: abc123def456`                        |
| `refs`            | Indicates this commit reverts another previous commit                                                                | `refs: abc123def456`                                |
| `implements`      | Indicates this commit implements a specific issue or feature request                                                   | `implements: #123`                                  |
| `resolves`        | Indicates this commit resolves a specific issue or feature request                                                          | `resolves: #123`                                    |

### Footer Example

```
feat(image-processing): add support for image resizing with OpenCV

This commit adds a new `ImageResizer` class that uses OpenCV to resize images while maintaining aspect ratio. The main motivation was to optimize image loading times in the application by allowing dynamic resizing based on device capabilities.
The implementation includes:
- New `ImageResizer` class with `resize()` method
- Integration with existing image processing pipeline in `ImageProcessor`
- Unit tests for `ImageResizer` in `ImageResizerTest`

BREAKING CHANGE: The `ImageProcessor` class now requires an `ImageResizer` instance in its constructor, which may break existing code that instantiates `ImageProcessor` without this dependency.

implements: #456
```

## 6. Complete Examples

### Example 1: Feature with behavior change

```
feat(auth): add support for OAuth2 login

Introduces a new `OAuth2LoginService` that allows users to authenticate using their Google accounts. This feature was requested to provide a more seamless login experience and reduce friction for new users.
Implements:
- `OAuth2LoginService` with methods for handling OAuth2 flow
- Integration with existing authentication system
Added Tests:
- `OAuth2LoginServiceTest` with unit tests for the new service

BREAKING CHANGE: The login endpoint now accepts additional parameters for OAuth2 authentication, which may require updates to client applications that consume this endpoint.

implements: #123

```

### Example 2: Bugfix with technical detail

```
fix(aut): correct integration with Google OAuth2 API

`GoogleOAuth2Client` was not properly handling token refresh, which caused authentication failures after the initial token expired. This commit refactors the token management logic to ensure that tokens are refreshed correctly and securely.
Changes include:
- Refactored `GoogleOAuth2Client` to include a `refreshToken()` method
- Updated `OAuth2LoginService` to call `refreshToken()` when necessary
Added and Updated Tests:
- `GoogleOAuth2ClientTest` with tests for token refresh logic
- Updated `OAuth2LoginServiceTest` to cover the new token refresh behavior

depends-on: abc123def456 (commit that introduced `OAuth2LoginService`)
resolves: #124
```

### Example 3: Documentation and chore

```
docs(auth): documents OAuth2 login flow and configuration

Adds a new section to _AUTHENTICATION.md_ detailing the OAuth2 login flow, configuration properties, and troubleshooting tips for common issues encountered during setup.
```

```
chore: update security dependencies

- Maven: 3.8.1 to 3.8.5
- Spring Security: 5.5 to 5.7.3
- JUnit: 5.7 to 5.9.2

```