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

### 详细参考（引用）

**自然语言示例 + Fallback + 组合流** → 见 `references/quick-reference.md`

### 快速决策速查

```
Web E2E 测试         → webapp-testing + playwright-cli
快速调试/截图        → playwright-cli
API 文档验证         → api-documenter + api-mock
性能测试             → performance-engineer
自动化测试规划       → test-automator
完成前验证           → verification-before-completion
Mock API             → api-mock
自动触发测试         → test-runner + run-tests-after-changes
Sprint/评审          → bmad-qa
未知任务             → bmad-qa + 询问澄清
```

---

## ❓ Fallback 处理

**当任务无法匹配任何规则时**：

```markdown
1. 询问用户澄清：
   "这个任务是 Web 测试、API 测试、还是其他？"

2. 如果用户无法描述：
   → bmad-qa（让 QA 核心帮你判断）
```

---

## 🔗 与 gql-qa 主 skill 联动

当 QA 角色加载 `qa-ext-skill` 时：

```markdown
1. 收到 QA 任务
2. 先加载 qa-ext-skill（路由器）
3. 根据任务路由到具体 skill
4. 执行完成后返回 bmad-qa 做评审
```

**注意**：`qa-ext-skill` 不会覆盖 `gql-qa` 主 skill，它们协同工作。

---

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 行数 |
|-------|-------|------|
| `bmad-qa.md` | QA 评审核心：Sprint 测试评审、缺陷报告 | 526 |
| `webapp-testing.md` | Playwright Python E2E：服务器管理、networkidle | 95 |
| `verification-before-completion.md` | QA 铁律：证据优先、禁止 should/Done | 139 |
| `playwright-cli.md` | CLI 工具：截图/DOM/cookie/route | 299 |
| `test-automator.md` | 测试自动化：TDD、AI 自愈、CI/CD | 203 |
| `performance-engineer.md` | 性能测试：k6/JMeter、Core Web Vitals | 150 |
| `api-documenter.md` | API 文档：OpenAPI 3.1+、Swagger、SDK | 161 |
| `test-runner.md` | 自动测试：PostToolUse 事件触发 | 28 |
| `api-mock.md` | Mock 服务器：FastAPI、场景管理 | 1335 |
| `run-tests-after-changes.md` | Git Hooks：编辑后自动测试 | 28 |

---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说 "测试" | 说明测试什么（Web？API？性能？） |
| 不验证就 claim 完成 | 先 `verification-before-completion` |
| Web 测试前不等待 networkidle | `page.wait_for_load_state('networkidle')` |
| API 测试不验证 OpenAPI | 用 `api-documenter` 先验证文档 |

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
