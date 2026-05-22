---
name: qa-ext-skill
description: QA 技能索引路由器 - 接收任何 QA 任务，智能推荐最合适的 skill 并执行
version: 2.0.0
author: relunctance
license: MIT
category: gql-bots
tags:
  - qa
  - testing
  - skill-router
  - gql-bots
  - intelligent-router
hermes:
  platforms:
    hermes: true
  auto_route: true
---

# QA Ext Skill - 智能技能路由器

> **核心定位**：QA 角色的中央路由器。任何 QA 任务进来，先查这里，再路由到具体 skill。

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "测试 contact 区块" | webapp-testing | `hermes -p qa -s webapp-testing` |
| "页面打不开，帮我看下" | playwright-cli | `hermes -p qa -s playwright-cli` |
| "验证 API 文档对不对" | api-documenter | `hermes -p qa -s api-documenter` |
| "做个性能测试" | performance-engineer | `hermes -p qa -s performance-engineer` |
| "写完代码了，怎么测试" | test-automator | `hermes -p qa -s test-automator` |
| "确保测试全部通过" | verification-before-completion | `hermes -p qa -s verification-before-completion` |
| "需要 Mock 一个 API" | api-mock | `hermes -p qa -s api-mock` |
| "代码改了，自动跑测试" | test-runner | `hermes -p qa -s test-runner` |
| "Sprint 测试评审" | bmad-qa | `hermes -p qa -s bmad-qa` |

### 一句话触发规则

```
任务包含...         → 直接路由到...
──────────────────────────────────────
"打不开"、"页面"、"截图"、"看不下" → playwright-cli
"Web"、"E2E"、"页面测试"、"contact"、"区块" → webapp-testing
"API"、"接口"、"文档"、"OpenAPI" → api-documenter
"性能"、"负载"、"压测"、"LCP" → performance-engineer
"自动化"、"写测试"、"TDD" → test-automator
"完成"、"通过"、"验证"、"确保" → verification-before-completion
"Mock"、"模拟"、"stub" → api-mock
"代码改"、"commit"、"push" → test-runner
"评审"、"Sprint"、"测试计划" → bmad-qa
```

---

## 🔀 智能路由决策树

```
收到 QA 任务
    │
    ├─ 包含 "Web" / "E2E" / "页面" / "contact" / "nav" / "hero" / "区块"
    │   └─→ webapp-testing + playwright-cli
    │
    ├─ 包含 "API" / "接口" / "OpenAPI" / "Swagger"
    │   └─→ api-documenter + api-mock
    │
    ├─ 包含 "性能" / "负载" / "压测" / "LCP" / "CLS"
    │   └─→ performance-engineer
    │
    ├─ 包含 "自动化" / "写测试" / "TDD" / "测试用例"
    │   └─→ test-automator
    │
    ├─ 包含 "Mock" / "模拟" / "stub" / "前后端并行"
    │   └─→ api-mock
    │
    ├─ 包含 "完成" / "通过" / "验证" / "确保" / "claim"
    │   └─→ verification-before-completion
    │
    ├─ 包含 "代码改" / "commit" / "push" / "变更"
    │   └─→ test-runner + run-tests-after-changes
    │
    ├─ 包含 "评审" / "Sprint" / "测试计划" / "DoD"
    │   └─→ bmad-qa
    │
    └─ 包含 "安全" / "XSS" / "注入"
        └─→ (参考 Review 角色: frontend-security-coder / backend-security-coder)
```

---

## 📋 技能地图

### 全部 Skill 一览

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| **bmad-qa** | QA 评审核心，处理 Sprint 测试评审、缺陷报告、测试报告 | P0 | 评审、Sprint、计划、缺陷报告 |
| **webapp-testing** | Playwright Python E2E 测试，服务器生命周期管理 | P0 | Web、E2E、页面测试、contact、nav |
| **verification-before-completion** | QA 铁律：完成前必须验证，证据优先 | P0 | 完成、通过、验证、确保、claim |
| **playwright-cli** | CLI 工具，快捷浏览器操作、截图、DOM 侦查 | P0 | 打不开、页面、截图、调试 |
| **test-automator** | 测试自动化规划，TDD/BDD，AI 测试 | P0 | 自动化、写测试、TDD、测试用例 |
| **performance-engineer** | 性能测试，k6/JMeter，Core Web Vitals | P1 | 性能、负载、压测、LCP、FID |
| **api-documenter** | OpenAPI 文档验证，SDK 生成 | P1 | API、接口、文档、OpenAPI |
| **test-runner** | 代码修改后自动运行相关测试 | P1 | 代码改、commit、变更 |
| **api-mock** | FastAPI Mock 服务器，场景管理 | P2 | Mock、模拟、stub、前后端并行 |
| **run-tests-after-changes** | Git Hooks 触发测试 | P2 | 代码改、编辑后测试 |

---

## 🎯 场景化深度参考

### 场景 1: Web E2E 测试

**任务示例**：
- "测试 contact 区块的表单提交"
- "验证 hero 区块的按钮点击"
- "Web 页面全流程测试"

**路由到**：
```
webapp-testing（主力）
  └─ playwright-cli（辅助调试）
```

**快速调用**：
```bash
hermes -p qa -s webapp-testing -q "测试 contact 区块表单提交"
```

**webapp-testing 关键能力**：
- ✅ Playwright Python 脚本
- ✅ 服务器生命周期管理（with_server.py）
- ✅ 静态 HTML vs 动态 Web 决策树
- ✅ 等待 networkidle 后再 DOM 侦查

**playwright-cli 关键能力**：
- ✅ `playwright-cli open <url>` 打开浏览器
- ✅ `playwright-cli snapshot` 获取页面快照
- ✅ `playwright-cli screenshot` 截图
- ✅ `playwright-cli route` 请求拦截/mock

---

### 场景 2: API 测试

**任务示例**：
- "验证 OpenAPI 文档是否正确"
- "测试 API 接口返回"
- "需要 Mock 一个后端 API"

**路由到**：
```
api-documenter（文档验证）
  └─ api-mock（接口 Mock）
```

**快速调用**：
```bash
hermes -p qa -s api-documenter -q "验证 OpenAPI 文档"
hermes -p qa -s api-mock -q "Mock /api/users 接口"
```

**api-documenter 关键能力**：
- ✅ OpenAPI 3.1+ 规范验证
- ✅ Swagger UI/Redoc 定制
- ✅ 文档驱动测试
- ✅ SDK 生成

**api-mock 关键能力**：
- ✅ FastAPI Mock 服务器
- ✅ 动态路由和请求匹配
- ✅ 场景切换（happy_path / error）
- ✅ Faker 数据生成

---

### 场景 3: 性能测试

**任务示例**：
- "做一下负载测试"
- "检查 LCP 和 CLS"
- "性能瓶颈在哪"

**路由到**：`performance-engineer`

**快速调用**：
```bash
hermes -p qa -s performance-engineer -q "执行负载测试"
```

**performance-engineer 关键能力**：
- ✅ k6/JMeter/Gatling 负载测试
- ✅ Core Web Vitals (LCP/FID/CLS)
- ✅ OpenTelemetry 可观测性
- ✅ 火焰图/内存分析

---

### 场景 4: 测试自动化

**任务示例**：
- "怎么用 TDD 开发"
- "帮我写测试用例"
- "搭建自动化测试框架"

**路由到**：
```
test-automator（主力）
  └─ verification-before-completion（验证）
```

**快速调用**：
```bash
hermes -p qa -s test-automator -q "设计测试自动化策略"
```

**test-automator 关键能力**：
- ✅ TDD 红绿重构循环
- ✅ AI 测试（自愈测试、智能定位）
- ✅ CI/CD 集成
- ✅ 混沌工程

---

### 场景 5: 完成前验证

**任务示例**：
- "确保测试全部通过"
- "验证代码能交付"
- "claim 完成后验证"

**路由到**：`verification-before-completion`

**快速调用**：
```bash
hermes -p qa -s verification-before-completion -q "验证所有测试通过"
```

**verification-before-completion 关键能力**：
- ✅ QA 铁律：证据优先于声明
- ✅ 识别验证命令 → 执行 → 读取输出 → 验证
- ✅ 红线：禁止 "should"、"probably"、"Done!"

---

### 场景 6: 浏览器调试

**任务示例**：
- "页面打不开，帮我看下"
- "截个图看看"
- "检查控制台错误"

**路由到**：`playwright-cli`

**快速调用**：
```bash
hermes -p qa -s playwright-cli -q "截图当前页面"
```

**playwright-cli 关键能力**：
- ✅ 浏览器截图
- ✅ DOM 快照（`.playwright-cli/` 目录）
- ✅ Cookie/LocalStorage 查看
- ✅ 请求拦截/mock

---

## 📖 References 快速索引

每个文件都有 TL;DR 摘要：

| 文件 | TL;DR | 行数 |
|------|-------|------|
| `references/bmad-qa.md` | QA 评审核心：Sprint 测试评审、缺陷报告、测试报告模板 | 526 |
| `references/webapp-testing.md` | Playwright Python E2E：服务器管理、决策树、networkidle 等待 | 95 |
| `references/verification-before-completion.md` | QA 铁律：完成前必须验证、证据优先、禁止 should/Done | 139 |
| `references/playwright-cli.md` | CLI 工具：open/goto/snapshot/click/route/screenshot | 299 |
| `references/test-automator.md` | 测试自动化：TDD/BDD、AI 自愈测试、CI/CD、混沌工程 | 203 |
| `references/performance-engineer.md` | 性能测试：k6/JMeter、Core Web Vitals、OpenTelemetry | 150 |
| `references/api-documenter.md` | API 文档：OpenAPI 3.1+、Swagger、SDK 生成 | 161 |
| `references/test-runner.md` | 自动测试：代码修改后触发、PostToolUse 事件 | 28 |
| `references/api-mock.md` | Mock 服务器：FastAPI、场景管理、Faker 数据 | 1335 |
| `references/run-tests-after-changes.md` | Git Hooks：编辑后自动测试 | 28 |

---

## 🔄 QA 工作流全景

```
用户: "测试 contact 区块"
    │
    ▼
qa-ext-skill（路由器）
    │  分析：包含 "contact" / "区块"
    ▼
webapp-testing
    │
    ├─→ verification-before-completion（每步验证）
    │
    ├─→ playwright-cli（调试时）
    │
    └─→ 生成测试报告

用户: "性能测试"
    │
    ▼
qa-ext-skill
    │  分析：包含 "性能"
    ▼
performance-engineer
    │
    └─→ k6 脚本 + 报告
```

---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说 "测试" | 说明测试什么（Web？API？性能？） |
| 不验证就 claim 完成 | 先 `verification-before-completion` |
| Web 测试前不等待 networkidle | `page.wait_for_load_state('networkidle')` |
| API 测试不验证 OpenAPI | 用 `api-documenter` 先验证文档 |

---

## 📚 完整文件路径

所有 skill 文件位于 `references/` 目录，使用时无需关心原始路径。

---

## 🔗 相关角色联动

| 任务类型 | 联动角色 | 说明 |
|---------|---------|------|
| 安全测试 | review | frontend-security-coder / backend-security-coder |
| 代码开发 | coder | bmad-dev + write-tests + tdd |
| 架构评审 | arc | bmad-architect |

---

## 升级说明

详见 `update_readme.md`
