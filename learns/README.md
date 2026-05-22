# QA Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### bmad-qa 路径

- **现象**: `profile_role_skill.md` 中写的是 `plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **实际位置**: `/home/gql/tmp/codebuddy-skills/plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **原因**: `profile_role_skill.md` 的路径已经包含了 `plugins/` 前缀
- **解决**: 拼接时注意不要重复加 `plugins/`

### playwright-cli 路径

- **现象**: `profile_role_skill.md` 中写的是 `plugins/playwright-cli/skills/playwright-cli/SKILL.md`
- **实际位置**: `/home/gql/tmp/codebuddy-skills/plugins/playwright-cli/skills/playwright-cli/SKILL.md`
- **注意**: 有两级 `playwright-cli` 目录层级

## #source-区分

### external_plugins vs plugins

- **问题**: skill 文件分布在两个目录
- **规则**:
  - `webapp-testing`, `superpowers/`, `unit-testing/`, `performance-testing-review/`, `api-testing-observability/`, `hooks-testing/` → `external_plugins/`
  - `agent-team-agile-workflow/`, `playwright-cli/` → `plugins/`

## #references-增强

### TL;DR 标记

- **目的**: 快速定位关键信息，AI 读取效率提升
- **格式**: `<!-- TL;DR: 一句话描述 -->`
- **位置**: 每个 reference 文件第一行
- **检查**: 遍历 references 时检查是否已有 TL;DR，避免重复添加

## #升级触发

### 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `gql-bots/shared/profile_role_skill.md` 变化时
3. `gql-bots/shared/skills-catalog.md` 更新时

## #智能路由

### 触发条件设计

- **auto_route**: true 标记启用自动路由
- **触发关键词**: 覆盖常见任务描述（Web、API、性能等）
- **决策树**: 清晰的 if-then 映射
- **快速参考表**: 一句话触发规则，便于快速查询

### 路由准确性保障

- 关键词要覆盖同义词（页面/区块/E2E/Web）
- 优先级：精准词 > 模糊词
- 必要时返回多个 skill（主力 + 辅助）
