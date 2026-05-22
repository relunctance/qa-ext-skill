# QA Ext Skill 升级方案

## 执行计划

详见 `qa-ext-skill-执行计划.md`（详细步骤说明）

## 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `gql-bots/shared/profile_role_skill.md` 变化时
3. `gql-bots/shared/skills-catalog.md` 更新时
4. QA 角色需要新能力时

## 升级步骤

### Step 1: 下载最新插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

### Step 2: 同步 references

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/qa-ext-skill/references

# P0 Skills
cp $PLUGINS/agent-team-agile-workflow/agents/bmad-qa.md $REFS/bmad-qa.md
cp $BASE/webapp-testing/SKILL.md $REFS/webapp-testing.md
cp $BASE/superpowers/skills/verification-before-completion/SKILL.md $REFS/verification-before-completion.md
cp $PLUGINS/playwright-cli/skills/playwright-cli/SKILL.md $REFS/playwright-cli.md
cp $BASE/unit-testing/agents/test-automator.md $REFS/test-automator.md

# P1 Skills
cp $BASE/performance-testing-review/agents/performance-engineer.md $REFS/performance-engineer.md
cp $BASE/api-testing-observability/agents/api-documenter.md $REFS/api-documenter.md
cp $BASE/hooks-testing/hooks/test-runner.md $REFS/test-runner.md

# P2 Skills
cp $BASE/api-testing-observability/commands/api-mock.md $REFS/api-mock.md
cp $BASE/hooks-testing/hooks/run-tests-after-changes.md $REFS/run-tests-after-changes.md
```

### Step 3: 更新 SKILL.md（如有变更）

如果 `skills-catalog.md` 更新，重新生成索引部分。

### Step 4: 更新 learns/

如有新踩坑，添加到 `learns/README.md`。

### Step 5: 提交

```bash
cd /home/gql/repos/qa-ext-skill
git add -A
git commit -m "chore: sync with latest codebuddy-plugins"
git push origin main
```

## 版本号规则

| 类型 | 规则 |
|------|------|
| 主版本 | skill 索引结构变化 |
| 次版本 | 新增/删除 skill |
| 修订版 | 内容更新 |

## 路径速查

| Skill | 源路径 |
|-------|--------|
| bmad-qa | `plugins/agent-team-agile-workflow/agents/bmad-qa.md` |
| webapp-testing | `external_plugins/webapp-testing/SKILL.md` |
| verification-before-completion | `external_plugins/superpowers/skills/verification-before-completion/SKILL.md` |
| playwright-cli | `plugins/playwright-cli/skills/playwright-cli/SKILL.md` |
| test-automator | `external_plugins/unit-testing/agents/test-automator.md` |
| performance-engineer | `external_plugins/performance-testing-review/agents/performance-engineer.md` |
| api-documenter | `external_plugins/api-testing-observability/agents/api-documenter.md` |
| test-runner | `external_plugins/hooks-testing/hooks/test-runner.md` |
| api-mock | `external_plugins/api-testing-observability/commands/api-mock.md` |
| run-tests-after-changes | `external_plugins/hooks-testing/hooks/run-tests-after-changes.md` |
