<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# QA 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

### Web 测试

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "帮我测试下 contact 区块能打开吗" | webapp-testing | contact 是 Web 区块 |
| "页面打不开，帮我看下" | playwright-cli | 调试场景 |
| "这个按钮点击没反应" | playwright-cli | DOM 侦查 |
| "帮我截个图看看" | playwright-cli | 截图 |
| "验证 hero 区块的轮播图" | webapp-testing | Web E2E |
| "页面报 500 了" | playwright-cli + api-documenter | 调试 + API 检查 |

### API 测试

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "帮我测试下这个 API 对不对" | api-documenter | API 验证 |
| "接口文档和实际不符" | api-documenter | 文档校验 |
| "后端没完成，先 Mock 一个" | api-mock | Mock |
| "需要切换到错误场景测试" | api-mock | 场景切换 |

### 性能测试

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "做一下负载测试" | performance-engineer | 负载 |
| "LCP 太慢了" | performance-engineer | Core Web Vitals |
| "帮我看看性能瓶颈" | performance-engineer | 分析 |

### 自动化测试

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "代码写完了，怎么写测试" | test-automator | 测试规划 |
| "帮我用 TDD 方式开发" | test-automator + tdd | TDD |
| "自动化测试覆盖不够" | test-automator | 覆盖率 |
| "代码改了，自动跑测试" | test-runner | CI 集成 |

### 完成验证

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "功能完成了，确保没问题" | verification-before-completion | 验证 |
| "帮我检查 DoD" | verification-before-completion | 规范检查 |
| "这个需求做好了，验收下" | verification-before-completion | 验收 |

### QA 评审

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "Sprint 快结束了，评审下测试" | bmad-qa | Sprint 评审 |
| "测试报告写一下" | bmad-qa | 报告 |
| "缺陷太多了，怎么管" | bmad-qa | 缺陷管理 |

---

## 🔄 Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是 Web 测试、API 测试、还是其他？"
    │
    └─ 如果用户无法描述：
        └─→ bmad-qa（让 QA 核心帮你判断）
```

**Fallback 规则**：
```markdown
无匹配时 → bmad-qa → 让他判断用哪个 skill
```

---

## 🔗 任务组合流

### 组合 1: Web 完整测试流程

```
"全面测试 contact 功能"
    │
    ├─ webapp-testing（E2E 测试）
    │     ├─ 表单验证
    │     ├─ 按钮交互
    │     └─ 数据提交
    │
    └─ verification-before-completion（完成验证）
          └─ 确保所有检查通过
```

**调用**：
```bash
# 先执行 E2E
hermes -p qa -s webapp-testing -q "测试 contact 表单"
# 完成后验证
hermes -p qa -s verification-before-completion -q "确保 contact 功能完成"
```

### 组合 2: API 调试 + Mock

```
"调试 /api/users 接口，Mock 它"
    │
    ├─ api-documenter（检查文档）
    │     └─ 确认接口规范
    │
    └─ api-mock（Mock 接口）
          ├─ 创建 /api/users mock
          └─ 切换场景测试
```

### 组合 3: 性能诊断

```
"页面加载慢，帮我分析"
    │
    ├─ playwright-cli（快速侦查）
    │     └─ 截图 + network 分析
    │
    └─ performance-engineer（深度分析）
          ├─ k6 负载测试
          └─ Core Web Vitals
```

### 组合 4: 测试自动化 + CI

```
"我要上 CI/CD"
    │
    ├─ test-automator（规划测试）
    │
    ├─ test-runner（自动运行）
    │
    └─ run-tests-after-changes（Git Hooks）
          └─ commit 时自动触发
```

---

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  任务类型              │  首选 Skill         │  辅助      │
├─────────────────────────────────────────────────────────────┤
│  Web E2E 测试         │  webapp-testing     │  playwright │
│  快速调试/截图         │  playwright-cli      │            │
│  API 文档验证          │  api-documenter     │  api-mock  │
│  性能测试              │  performance-eng.   │            │
│  自动化测试规划        │  test-automator     │  write-tests│
│  完成前验证            │  verification-bef.   │            │
│  Mock API              │  api-mock           │            │
│  自动触发测试          │  test-runner        │  run-tests │
│  Sprint/评审           │  bmad-qa            │            │
│  未知任务              │  bmad-qa            │  询问澄清  │
└─────────────────────────────────────────────────────────────┘
```
