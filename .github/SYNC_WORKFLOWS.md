# Sincronização de Labels e Milestones

Este documento descreve como usar os workflows de sincronização de labels e milestones entre repositórios da organização Logika-Sciuro.

## Visão Geral

Existem dois workflows principais:

- **Sync Repository Labels** — sincroniza labels entre repositórios
- **Sync Sprints (Milestones)** — sincroniza milestones entre repositórios

Ambos suportam três modos de operação:

1. **Push automático** — triggered ao fazer merge em `main`, sempre em modo produção (sem dry-run)
2. **Workflow Dispatch (Dry-Run)** — teste sem alterar repositórios
3. **Workflow Dispatch (Produção)** — aplicar sincronização em repositórios reais

## Modo Dry-Run (Teste Seguro)

Use este modo para **validar mudanças antes de aplicar** em produção, sem escrever em nenhum repositório.

### Sync Repository Labels (Dry-Run)

1. Acesse: `Actions` → `Sync Repository Labels`
2. Clique em **"Run workflow"**
3. Configure:
   - **target_env**: `sandbox` (padrão)
   - **dry_run**: `true` (padrão)
   - **repositories**: _(opcional)_ liste repositórios para testar, ex: `scavenger-infra-as-code, scavenger-core-gateway`
4. Clique em **"Run workflow"**

**Resultado esperado:**
- O workflow valida se os labels do `.github` estão presentes nos repositórios alvo
- Se houver diferenças (faltando, sobrando, cor/descrição diferentes), reporta warnings
- **Nenhuma escrita é feita**
- Status: ✓ success (sem drift) ou ✗ failure (com drift detectado)

**Exemplo de output:**
```
Dry-run validation passed for Logika-Sciuro/scavenger-infra-as-code.
Dry-run validation passed for Logika-Sciuro/scavenger-core-gateway.
```

Ou com drift:
```
::warning::Missing label 'type: feature' in Logika-Sciuro/scavenger-infra-as-code.
::warning::Extra labels found in Logika-Sciuro/scavenger-infra-as-code:
bug
documentation
```

### Sync Sprints (Milestones) (Dry-Run)

1. Acesse: `Actions` → `Sync Sprints (Milestones)`
2. Clique em **"Run workflow"**
3. Configure:
   - **target_env**: `sandbox` (padrão)
   - **dry_run**: `true` (padrão)
   - **repositories**: _(opcional)_ ex: `scavenger-infra-as-code`
4. Clique em **"Run workflow"**

**Resultado esperado:**
- O workflow valida se as milestones (Sprint 1/2/3) existem e estão sincronizadas
- Reporta warnings se houver diferenças em título, data, descrição ou estado
- **Nenhuma escrita é feita**

**Exemplo de output:**
```
Milestone 'Sprint 1 - Fundação & Mocking Criptográfico' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
Milestone 'Sprint 2 - Caminho Quente (Multiplexação gRPC)' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
Milestone 'Sprint 3 - Caminho Frio & Validação E2E' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
```

## Modo Produção

Use este modo para **sincronizar labels e milestones reais** nos repositórios da organização.

### Sync Repository Labels (Produção)

1. Acesse: `Actions` → `Sync Repository Labels`
2. Clique em **"Run workflow"**
3. Configure:
   - **target_env**: `prod`
   - **dry_run**: `false`
   - **repositories**: _(deixe vazio para usar repositórios padrão)_
4. Clique em **"Run workflow"**

**Resultado esperado:**
- O workflow sincroniza todos os labels do `.github` para os repositórios alvo
- Remove labels extras (não presentes em `.github/labels.yml`)
- Atualiza cores e descrições de labels existentes
- Status: ✓ success após sincronização

**Repositórios padrão:**
- `scavenger-infra-as-code`
- `scavenger-core-gateway`
- `scavenger-cv-engine`
- `scavenger-e2e-framework`

### Sync Sprints (Milestones) (Produção)

1. Acesse: `Actions` → `Sync Sprints (Milestones)`
2. Clique em **"Run workflow"**
3. Configure:
   - **target_env**: `prod`
   - **dry_run**: `false`
   - **repositories**: _(deixe vazio para usar repositórios padrão)_
4. Clique em **"Run workflow"**

**Resultado esperado:**
- O workflow cria ou atualiza as 3 milestones de sprint:
  - Sprint 1 - Fundação & Mocking Criptográfico (due: 2026-05-21)
  - Sprint 2 - Caminho Quente (Multiplexação gRPC) (due: 2026-05-28)
  - Sprint 3 - Caminho Frio & Validação E2E (due: 2026-06-04)
- Status: ✓ success após sincronização

## Fluxo Recomendado

### Para Desenvolvimento/Teste

1. Modifique `.github/labels.yml` ou o workflow de milestones localmente
2. Crie PR e solicite review
3. **Antes de fazer merge**, execute dry-run na branch:
   - Dispatch com `target_env=prod`, `dry_run=true`
   - Verifique se não há warnings inesperados
4. Após aprovação, faça merge em `main`
5. O workflow de produção executa automaticamente (sem dry-run)

### Para Sincronizar Rapidamente

Se você já sabe que quer sincronizar e os testes passaram:

1. Dispatch com `target_env=prod`, `dry_run=false`
2. Aguarde conclusão
3. Verifique um repositório manualmente:
   ```bash
   gh label list -R Logika-Sciuro/scavenger-infra-as-code
   gh api /repos/Logika-Sciuro/scavenger-infra-as-code/milestones --jq '.[].title'
   ```

## Referência de Inputs

| Input | Tipo | Default | Descrição |
|-------|------|---------|-----------|
| `target_env` | choice | `sandbox` | Ambiente alvo: `sandbox` ou `prod` |
| `dry_run` | boolean | `true` | Validar sem escrever |
| `repositories` | string | _(vazio)_ | Repositórios (separados por vírgula, sem owner). Se vazio e `target_env=prod`, usa padrão. Se vazio e `target_env=sandbox`, requer input. |

## Troubleshooting

### Erro: "No repositories resolved"

**Causa:** Executou com `target_env=sandbox` sem fornecer `repositories`.

**Solução:** Forneça lista de repositórios em `repositories`, ex: `sandbox-labels-test, my-test-repo`.

### Dry-run detecta drift, mas produção falha

**Causa:** Pode haver mudanças não commitadas ou workflow não atualizado no remoto.

**Solução:**
1. Verifique se a branch está atualizada: `git pull origin main`
2. Confirme que as mudanças estão em `.github/labels.yml`
3. Reexecute o dry-run

### Labels não aparecem após produção

**Causa:** Token sem permissões adequadas ou rate-limit.

**Solução:**
1. Confirme que `ORG_PAT_ISSUES_WRITE` tem escopos: `repo`, `issues`
2. Aguarde alguns minutos e tente novamente
3. Verifique permissões do repositório alvo: Organization owner deve ter acesso

### Milestones não sincronizaram corretamente

**Causa:** Formatos de data ou estado incompatíveis.

**Solução:**
1. Verifique as datas esperadas em `sync-sprints-milestones.yml`
2. Valide via API:
   ```bash
   gh api /repos/Logika-Sciuro/scavenger-infra-as-code/milestones --jq '.[] | {title, state, due_on}'
   ```

## Links Úteis

- [Workflow: Sync Repository Labels](.github/workflows/sync-labels.yml)
- [Workflow: Sync Sprints (Milestones)](.github/workflows/sync-sprints-milestones.yml)
- [Labels Config](.github/labels.yml)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
