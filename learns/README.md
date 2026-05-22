# QA Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### bmad-qa 路径陷阱

**问题**：`profile_role_skill.md` 中写的是 `plugins/agent-team-agile-workflow/agents/bmad-qa.md`，但拼接时要注意路径层级。

**原因**：profile_role_skill.md 中的路径已经包含了 `plugins/` 前缀。

**解决**：拼接时直接用 `$PLUGINS/agent-team-agile-workflow/agents/bmad-qa.md`。

### playwright-cli 路径陷阱

**问题**：有两个 `playwright-cli` 目录层级。

**原因**：`plugins/playwright-cli/skills/playwright-cli/SKILL.md` 有两级 playwright-cli。

**解决**：确保路径正确复制。

## #source-区分

### external_plugins vs plugins

**问题**：skill 文件分布在两个目录。

**规则**：
- `webapp-testing`, `superpowers/`, `unit-testing/`, `performance-testing-review/`, `api-testing-observability/`, `hooks-testing/` → `external_plugins/`
- `agent-team-agile-workflow/`, `playwright-cli/` → `plugins/`

## #升级触发

### 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `profile_role_skill.md` 变化时
3. `skills-catalog.md` 更新时
