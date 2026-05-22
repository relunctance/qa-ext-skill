# QA Ext Skill 创建执行计划

## 目标

基于 `profile_role_skill.md` 和 `skills-catalog.md`，创建智能技能路由器仓库 `qa-ext-skill`，包含：
- ✅ 智能索引推荐功能（决策树 + 触发关键词 + 快速参考表）
- ✅ 所有 QA skill 的 references（含 TL;DR 摘要）
- ✅ 升级方案
- ✅ learns 踩坑记录

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
gh repo view relunctance/qa-ext-skill 2>/dev/null && echo "EXISTS" || echo "NOT_EXISTS"
```

- **如果不存在**：使用 `skill-created` 创建
- **如果存在**：直接使用，验证后更新

---

## Step 3: 复制 profile_role_skill.md

```bash
cp /home/gql/repos/gql-bots/shared/profile_role_skill.md /tmp/profile_role_skill.md
```

**QA 角色 skill 列表（来源：`/home/gql/repos/gql-bots/shared/profile_role_skill.md`）**：

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

**QA 技能地图（来源：`/home/gql/repos/gql-bots/shared/skills-catalog.md`）**：

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

---

## Step 7: 创建 qa-ext-skill 仓库

**目录结构**：

```
qa-ext-skill/
├── SKILL.md                   # 智能路由器（核心）
├── README.md                  # 仓库说明
├── LICENSE                   # MIT
├── update_readme.md          # 升级方案
├── qa-ext-skill-执行计划.md  # 执行计划
├── learns/                   # 踩坑记录
│   └── README.md
└── references/               # skill 文件副本（含TL;DR）
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

### 7.1 SKILL.md 设计要点（超级重要）

**必须包含**：

```markdown
---
name: qa-ext-skill
description: QA 技能索引路由器 - 接收任何 QA 任务，智能推荐最合适的 skill 并执行
version: 2.0.0  # 重大更新
hermes:
  auto_route: true  # 启用自动路由
---

# QA Ext Skill - 智能技能路由器

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "测试 contact 区块" | webapp-testing | `hermes -p qa -s webapp-testing` |
| ... | ... | ... |

### 一句话触发规则

```
任务包含...         → 直接路由到...
──────────────────────────────────────
"打不开"、"页面"、"截图" → playwright-cli
"Web"、"E2E"、"页面测试" → webapp-testing
...
```

## 🔀 智能路由决策树

```
收到 QA 任务
    │
    ├─ 包含 "Web" / "E2E" / "页面"
    │   └─→ webapp-testing + playwright-cli
    │
    ...
```

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-qa | QA评审核心 | P0 | 评审、Sprint |

## 🎯 场景化深度参考

### 场景 1: Web E2E 测试
...
```

### 7.2 references 添加 TL;DR

```bash
# 每个 reference 文件第一行添加 TL;DR
for f in references/*.md; do
  case "$f" in
    bmad-qa.md)
      echo "<!-- TL;DR: QA评审核心：Sprint测试评审、缺陷报告 -->" | cat - "$f" > temp && mv temp "$f"
      ;;
    webapp-testing.md)
      echo "<!-- TL;DR: Playwright Python E2E：服务器管理 -->" | cat - "$f" > temp && mv temp "$f"
      ;;
    ...
  esac
done
```

### 7.3 references 复制命令

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

**learns/README.md 模板**：

```markdown
# QA Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### bmad-qa 路径
- **现象**: profile_role_skill.md 中写的是 `plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **实际位置**: `/home/gql/tmp/codebuddy-skills/plugins/agent-team-agile-workflow/agents/bmad-qa.md`
- **原因**: profile_role_skill.md 的路径已经包含了 `plugins/` 前缀
- **解决**: 拼接时注意不要重复加 `plugins/`

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

## #智能路由

### 触发条件设计
- **auto_route**: true 标记启用自动路由
- **触发关键词**: 覆盖常见任务描述（Web、API、性能等）
- **决策树**: 清晰的 if-then 映射
```

---

## Step 9: 创建 update_readme.md 升级方案

```markdown
# QA Ext Skill 升级方案

## 执行计划

详见 `qa-ext-skill-执行计划.md`（详细步骤说明）

## 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `gql-bots/shared/profile_role_skill.md` 变化时
3. `gql-bots/shared/skills-catalog.md` 更新时

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

# 复制所有 skill 文件
# [同 Step 7.3 的复制命令]
```

### Step 3: 重新添加 TL;DR（如有需要）

```bash
# 检查并添加 TL;DR
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    # 添加 TL;DR
    ...
  fi
done
```

### Step 4: 提交

```bash
cd /home/gql/repos/qa-ext-skill
git add -A
git commit -m "chore: sync with latest codebuddy-plugins"
git push origin main
```

## 版本号规则

| 类型 | 规则 |
|------|------|
| 主版本 | skill 索引结构变化、决策树重构 |
| 次版本 | 新增/删除 skill、触发关键词更新 |
| 修订版 | 内容更新、TL;DR 更新 |

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
```

---

## Step 10: 更新 README.md

使用 `readme-skill` 美化 README，包含：
- 5 个徽章（包括 auto_route）
- 目录结构
- 一句话路由规则
- 决策树
- TL;DR 索引表

---

## Step 11: Git 提交

```bash
cd /home/gql/repos/qa-ext-skill
git add -A
git commit -m "feat: v2.0 - intelligent skill router with auto_route

- 增强 SKILL.md: 决策树 + 触发关键词 + 快速参考表
- references 添加 TL;DR 摘要
- 场景化深度参考
- 智能路由决策树
- auto_route: true 启用自动路由"
git push origin main
```

---

## 验证清单

- [ ] 下载解压成功
- [ ] GitHub 仓库已创建/更新
- [ ] 所有 10 个 skill 路径验证通过
- [ ] references/ 包含所有 skill 文件（含 TL;DR）
- [ ] SKILL.md 包含智能索引：
  - [ ] 一句话触发规则
  - [ ] 决策树
  - [ ] 技能地图
  - [ ] 场景化深度参考
  - [ ] TL;DR 索引表
- [ ] learns/ 有踩坑记录
- [ ] update_readme.md 有升级方案
- [ ] README.md 已美化
- [ ] auto_route: true 启用
- [ ] git push 成功

---

## 其他角色创建注意事项

创建其他角色（arc-ext-skill / sm-ext-skill / coder-ext-skill / review-ext-skill / leader-ext-skill）时，参考以下模板：

### 1. SKILL.md 必须包含

```markdown
## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| ... | ... | ... |

### 一句话触发规则

```
任务包含...         → 直接路由到...
```

## 🔀 智能路由决策树

```
收到任务
    │
    ├─ 包含 "X"
    │   └─→ skill-a + skill-b
    │
    ...
```

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|

## 🎯 场景化深度参考

### 场景 1: ...
```

### 2. references 添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    echo "<!-- TL;DR: 一句话描述 -->" | cat - "$f" > temp && mv temp "$f"
  fi
done
```

### 3. 路径速查（更新 document 中的路径）

确保 `update_readme.md` 中的路径速查表是最新的。
