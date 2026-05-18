---
# description: 
# applyTo: 'Describe when these instructions should be loaded by the agent based on task context' # when provided, instructions will automatically be added to the request context when the pattern matches an attached file
---

<!-- Tip: Use /create-instructions in chat to generate content with agent assistance -->


<!-- This file should contain repository-specific guidance used by the PR review agent.
Keep test commands, branch policies, PR template expectations and any project
checklists here. The agent's action flow lives in `.agent.md` and will reference
the rules and commands defined in this file. -->

<!-- -- Repository customization checklist (edit these sections for your project) -- -->

Project name: 
Repository owner: 
Default branch: 
CI provider: GitHub Actions

1. Acceptance criteria template
<!-- Describe the minimal acceptance criteria for PRs in this repository. -->

2. Branching policy
<!-- - Define required branch name patterns and protected branch rules here. -->

3. PR template expectations
<!-- - List fields in the PR template that must be filled (e.g., Motivation, Changes, How to test, Screenshots) -->

4. Critical components and layers
<!-- Map repository folders to conceptual layers (e.g., `src/domain` → `domain`, `src/infra` → `infra`). -->

5. Test coverage policy
<!-- State expectations: unit tests required, integration tests required, coverage thresholds, etc. -->

6. Documentation rules
<!-- Document where architectural docs live and expectations for updating them on breaking changes. -->
