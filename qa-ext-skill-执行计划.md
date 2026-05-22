# QA Ext Skill 创建执行计划

## 目标
基于 `profile_role_skill.md` 和 `skills-catalog.md`，创建 `qa-ext-skill` GitHub 仓库，包含：
- 智能索引推荐功能
- 所有 QA skill 的 references
- 升级方案
- learns 踩坑记录

---

## Step 1: 下载 & 解压插件包

```bash
# 下载最新插件包
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip

# 解压到 /home/gql/tmp/codebuddy-skills（目录存在则跳过）
mkdir -p /home/gql/tmp/codebuddy-skills
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

**验证**：
```bash
ls /home/gql/tmp/codebuddy-skills/
# 应有: external_plugins/  plugins/  dist/
```

---

## Step 2: 确认 GitHub 仓库

仓库名：`qa-ext-skill`

```bash
# 检查是否已存在
gh repo view gql-tseng/qa-ext-skill 2>/dev/null && echo "EXISTS" || echo "NOT_EXISTS"
```

- **如果不存在**：使用 `skill-created` 创建
- **如果存在**：直接使用，验证后更新

---

## Step 3: 复制 profile_role_skill.md

```bash
cp /home/gql/repos/gql-bots/shared/profile_role_skill.md /tmp/profile_role_skill.md
```

**QA 角色 skill 列表（从 profile_role_skill.md）**：

| Skill | 路径 | 级别 |
|-------|------|------|
| bmad-qa | `plugins/agent-team-agile-workflow/agents/bmad-qa.md` | P0 |
| webapp-testing | `webapp-testing/SKILL.md` | P0 |
| verification-before-completion | `superpowers/skills/verification-before-completion/SKILL.md` | P0 |
| playwright-cli | `plugins/playwright-cli/skills/playwright-cli/SKILL.md` | P0 |
| test-automator | `unit-testing/agents/test-automator.md` | P0 |
| performance-engineer | `performance-testing-review/agents/performance-engineer.md` | P1 |
| api-documenter | `api-testing-observability/agents/api-documenter.md` | P1 |
| test-runner | `hooks-testing/hooks/test-runner.md` | P1 |
| api-mock | `api-testing-observability/commands/api-mock.md` | P2 |
| run-tests-after-changes | `hooks-testing/hooks/run-tests-after-changes.md` | P2 |

---

## Step 4: 确认 skill 路径存在

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins

# 逐个验证（全部应该存在）
for skill in \
  "agent-team-agile-workflow/agents/bmad-qa.md" \
  "webapp-testing/SKILL.md" \
  "superpowers/skills/verification-before-completion/SKILL.md" \
  "plugins/playwright-cli/skills/playwright-cli/SKILL.md" \
  "unit-testing/agents/test-automator.md" \
  "performance-testing-review/agents/performance-engineer.md" \
  "api-testing-observability/agents/api-documenter.md" \
  "hooks-testing/hooks/test-runner.md" \
  "api-testing-observability/commands/api-mock.md" \
  "hooks-testing/hooks/run-tests-after-changes.md"
do
  if [ -f "$PLUGINS/$skill" ] || [ -f "$BASE/$skill" ]; then
    echo "✅ $skill"
  else
    echo "❌ $skill NOT FOUND"
  fi
done
```

---

## Step 5: 阅读 qa_update.md 获取上下文

```bash
cat /home/gql/repos/gql-bots/docs/roles_skill/qa_update.md
```

**关键信息提取**：
- QA 角色的核心痛点（测试覆盖不全、缺乏自动化、缺少性能/API 测试）
- 每个 skill 的能力描述
- 决策树和工作流

---

## Step 6: 阅读 skills-catalog.md 理解技能地图

```bash
cat /home/gql/repos/gql-bots/shared/skills-catalog.md
```

**QA 技能地图**：

| 场景 | 推荐 Skill | 理由 |
|------|-----------|------|
| QA 评审 | bmad-qa | QA 评审核心 |
| Web 测试 | webapp-testing | Web 测试核心能力 |
| 完成前验证 | verification-before-completion | QA 铁律，避免漏测 |
| 快速调试验证 | playwright-cli | 快速调试与验证 |
| 测试自动化 | test-automator | 测试自动化基础 |
| 性能测试 | performance-engineer | 性能测试 |
| API 文档验证 | api-documenter | API 文档验证 |
| 测试运行自动化 | test-runner | 测试运行自动化 |
| 前后端并行开发 | api-mock | 前后端并行开发 |
| 代码变更测试 | run-tests-after-changes | 代码变更测试 |

**QA 工作流**：

```
接到测试任务
  │
  ├─→ bmad-qa → 执行 QA 评审规范
  │
  ├─→ verification-before-completion → 完成任务前必须验证
  │
  ├─→ webapp-testing → Web 功能测试
  │     └─→ playwright-cli → 快速调试
  │
  ├─→ test-automator → 测试自动化
  │     ├─→ write-tests → 编写测试
  │     └─→ tdd → TDD 流程
  │
  └─→ performance-engineer → 性能测试
```

---

## Step 7: 创建/更新 qa-ext-skill 仓库

**目录结构**：

```
qa-ext-skill/
├── SKILL.md              # 技能索引（智能推荐）
├── README.md             # 仓库说明
├── update_readme.md     # 升级方案
└── references/          # skill 文件副本
    ├── bmad-qa.md
    ├── webapp-testing.md
    ├── verification-before-completion.md
    ├── playwright-cli.md
    ├── test-automator.md
    ├── performance-engineer.md
    ├── api-documenter.md
    ├── test-runner.md
    ├── api-mock.md
    └── run-tests-after-changes.md
```

**SKILL.md 结构**：

```markdown
---
name: qa-ext-skill
description: QA 技能索引 - 根据任务类型推荐合适的 QA skill
version: 1.0
trigger:
  command: kanban create --skill qa-ext-skill
  inputs:
    - 任务类型
    - 任务描述
hermes:
  tags: [qa, skill-router]
---

# QA Ext Skill - 技能索引

## 快速参考表

| 场景 | 推荐 Skill | 适用角色 |
|------|-----------|---------|
| QA 评审 | bmad-qa | QA |
| Web 测试 | webapp-testing + playwright-cli | QA |
| 完成前验证 | verification-before-completion | QA |
| 测试自动化 | test-automator | QA |
| 性能测试 | performance-engineer | QA |
| API 文档验证 | api-documenter | QA |
| 测试运行 | test-runner | QA |
| API Mock | api-mock | QA |
| 变更测试 | run-tests-after-changes | QA |

## 技能地图

[详细内容来自 skills-catalog.md]

## 工作流

[详细内容来自 qa_update.md]

## References

所有 skill 文件位于 `references/` 目录
```

**references 复制命令**：

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/qa-ext-skill/references

mkdir -p $REFS

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

---

## Step 8: 创建 learns/ 踩坑记录

```bash
mkdir -p /home/gql/repos/qa-ext-skill/learns
```

**learns/01-qa路径确认.md**：

```markdown
# QA Skill 路径踩坑记录

## 问题1: bmad-qa 路径

- **现象**: profile_role_skill.md 中写的是 `plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **实际位置**: `/home/gql/tmp/codebuddy-skills/plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **原因**: profile_role_skill.md 的路径已经包含了 `plugins/` 前缀
- **解决**: 拼接时注意不要重复加 `plugins/`

## 问题2: playwright-cli 路径

- **现象**: profile_role_skill.md 中写的是 `plugins/playwright-cli/skills/playwright-cli/SKILL.md`
- **实际位置**: `/home/gql/tmp/codebuddy-skills/plugins/playwright-cli/skills/playwright-cli/SKILL.md`
- **注意**: 有两个 `playwright-cli` 目录层级
```

---

## Step 9: 创建 update_readme.md 升级方案

```bash
cat > /home/gql/repos/qa-ext-skill/update_readme.md << 'EOF'
# QA Ext Skill 升级方案

## 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. gql-bots 的 `profile_role_skill.md` 变化时
3. skills-catalog.md 更新时

## 升级步骤

### 1. 下载最新插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

### 2. 同步 references

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/qa-ext-skill/references

# 重新复制所有 skill 文件
# [同 Step 7 的复制命令]
```

### 3. 更新 SKILL.md

如果 `skills-catalog.md` 更新，重新生成索引部分。

### 4. 提交

```bash
cd /home/gql/repos/qa-ext-skill
git add -A
git commit -m "chore: sync with latest codebuddy-plugins"
git push origin main
```

## 版本号规则

- 主版本：skill 索引结构变化
- 次版本：新增/删除 skill
- 修订版：内容更新
EOF
```

---

## Step 10: 验证 & 更新 README.md

使用 `readme-skill` 美化 README，包含：
- 徽章
- 目录
- 使用说明
- 工作流图

---

## Step 11: 后续行为

- 美化 README（使用 readme-skill）
- 更新 gql-skills（如需要）

---

## Step 12: 提交

```bash
cd /home/gql/repos/qa-ext-skill
git add -A
git commit -m "feat: initial qa-ext-skill with all QA skills"
git push origin main
```

---

## 验证清单

- [ ] 下载解压成功
- [ ] GitHub 仓库已创建/更新
- [ ] 所有 10 个 skill 路径验证通过
- [ ] references/ 包含所有 skill 文件
- [ ] SKILL.md 包含智能索引
- [ ] learns/ 有踩坑记录
- [ ] update_readme.md 有升级方案
- [ ] README.md 已美化
- [ ] git push 成功
