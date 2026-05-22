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

---

## ⚡ TL;DR 速查索引

| 你要做的事 | 直接路由 | 说明 |
|------------|---------|------|
| Web 测试/E2E | webapp-testing | Playwright |
| 页面打不开/截图 | playwright-cli | 快速调试 |
| API 文档验证 | api-documenter | OpenAPI |
| 性能测试 | performance-engineer | k6/JMeter |
| 写自动化测试 | test-automator | TDD/BDD |
| 确保完成/验证 | verification-before-completion | QA铁律 |
| Mock API | api-mock | 前后端并行 |
| 代码改自动测试 | test-runner | 触发测试 |
| Sprint 评审 | bmad-qa | 评审核心 |
| 安全测试 | review | frontend/backend-security |
| 不知道用哪个 | bmad-qa | 让它帮你判断 |

---

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

### 一句话触发规则（增强版）

```
任务包含...              → 直接路由到...
────────────────────────────────────────────────────────────────────────────
# Web 测试
"Web"、"E2E"、"页面测试" → webapp-testing
"contact"、"nav"、"hero"、"区块" → webapp-testing
"打不开"、"截图"、"调试" → playwright-cli
"DOM"、"cookie"、"route" → playwright-cli

# API 测试
"API"、"接口"、"OpenAPI"、"Swagger" → api-documenter
"Mock"、"模拟"、"stub" → api-mock
"前后端并行" → api-mock
"请求"、"响应"、"json" → api-documenter

# 性能测试
"性能"、"负载"、"压测" → performance-engineer
"k6"、"jmeter"、"gatling" → performance-engineer
"LCP"、"FID"、"CLS"、"Core Web Vitals" → performance-engineer
"响应时间"、"TTFB"、"包大小" → performance-engineer

# 自动化测试
"自动化"、"写测试"、"TDD"、"BDD" → test-automator
"测试用例"、"用例设计" → test-automator
"AI 测试"、"自愈" → test-automator
"CI/CD"、"持续集成" → test-automator

# 验证铁律
"完成"、"通过"、"验证"、"确保"、"claim" → verification-before-completion
"自测"、"自验证" → verification-before-completion

# 自动触发
"代码改"、"commit"、"push" → test-runner
"变更"、"编辑后测试" → run-tests-after-changes

# 评审/计划
"评审"、"Sprint"、"测试计划"、"DoD" → bmad-qa
"缺陷"、"bug 报告" → bmad-qa

# 安全测试
"安全"、"XSS"、"注入" → (参考 Review 角色)
```

---

## 🔀 智能路由决策树

```
收到 QA 任务
    │
    ├─ 🎯 Web 测试？
    │   ├─ "打不开"/"截图" → playwright-cli
    │   └─ "contact"/"区块"/"E2E" → webapp-testing + playwright-cli
    │
    ├─ 🎯 API 测试？
    │   ├─ 文档验证 → api-documenter
    │   └─ Mock/模拟 → api-mock
    │
    ├─ 🎯 性能测试？
    │   └─ performance-engineer
    │         ├─ LCP/FID/CLS → Core Web Vitals
    │         └─ 负载/压测 → k6/JMeter
    │
    ├─ 🎯 自动化测试？
    │   └─ test-automator
    │         ├─ TDD/BDD
    │         └─ AI 自愈
    │
    ├─ 🎯 验证铁律？
    │   └─ verification-before-completion
    │
    ├─ 🎯 自动触发？
    │   └─ test-runner + run-tests-after-changes
    │
    ├─ 🎯 评审/计划？
    │   └─ bmad-qa
    │
    └─ ❓ 不知道
        └─ bmad-qa + 询问澄清
```

---

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-qa | QA 评审核心：Sprint 测试评审、缺陷报告 | P0 | 评审、Sprint、计划、缺陷报告 |
| webapp-testing | Playwright E2E：页面测试、contact、nav | P0 | Web、E2E、页面测试、contact |
| verification-before-completion | QA 铁律：完成前必须验证 | P0 | 完成、通过、验证、确保 |
| playwright-cli | CLI 工具：截图、DOM、cookie、route | P0 | 打不开、截图、调试 |
| test-automator | 测试自动化：TDD/BDD、AI 测试 | P0 | 自动化、写测试、TDD |
| performance-engineer | 性能测试：k6/JMeter、Core Web Vitals | P1 | 性能、负载、压测、LCP |
| api-documenter | API 文档：OpenAPI、Swagger | P1 | API、接口、文档、OpenAPI |
| test-runner | 自动测试：PostToolUse 事件触发 | P1 | 代码改、commit、变更 |
| api-mock | Mock 服务器：FastAPI、场景管理 | P2 | Mock、模拟、stub |
| run-tests-after-changes | Git Hooks：编辑后自动测试 | P2 | 代码改、编辑后测试 |

---

## 🎯 场景化深度参考（4大场景）

### 场景 1: Web E2E 测试 🔍

```
需求：测试 contact 区块
    │
    ├─ 快速调试/截图
    │   └─ playwright-cli
    │         → page.screenshot()
    │         → page.locator().click()
    │
    └─ 完整 E2E 测试
        └─ webapp-testing
              → page.wait_for_load_state('networkidle')
              → 页面交互
              → 断言验证
```

### 场景 2: API 测试 🔧

```
需求：验证 API 文档并测试
    │
    ├─ 文档验证
    │   └─ api-documenter
    │         → OpenAPI 3.1+
    │         → Swagger 验证
    │
    └─ Mock 测试
        └─ api-mock
              → FastAPI Mock
              → 场景管理
              → 前后端并行开发
```

### 场景 3: 性能测试 ⚡

```
需求：做性能测试
    │
    └─ performance-engineer
          ├─ 负载测试
          │     → k6/JMeter
          │     → TPS/VUS
          │
          ├─ Core Web Vitals
          │     → LCP（加载性能）
          │     → FID（交互性）
          │     → CLS（视觉稳定性）
          │
          └─ 响应时间分析
                → TTFB
                → 包大小
```

### 场景 4: 自动化测试 🤖

```
需求：写自动化测试
    │
    └─ test-automator
          ├─ TDD 流程
          │     → 先写测试
          │     → 再写代码
          │
          ├─ BDD 流程
          │     → Given-When-Then
          │     → 业务场景
          │
          └─ AI 自愈
                → 智能重试
                → 元素定位修复
```

### 快速决策速查

```
┌────────────────────────────────────────────────────────────┐
│  场景              │  路由顺序                              │
├────────────────────────────────────────────────────────────┤
│  Web E2E 测试       │  webapp-testing + playwright-cli     │
│  快速调试/截图      │  playwright-cli                      │
│  API 文档验证       │  api-documenter + api-mock           │
│  性能测试           │  performance-engineer                 │
│  自动化测试规划     │  test-automator                      │
│  完成前验证         │  verification-before-completion      │
│  Mock API           │  api-mock                           │
│  自动触发测试       │  test-runner + run-tests-after-changes│
│  Sprint/评审        │  bmad-qa                            │
│  安全测试           │  review-ext-skill (frontend-sec)    │
│  未知任务           │  bmad-qa + 询问澄清                 │
└────────────────────────────────────────────────────────────┘
```

---

## ❓ Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是 Web 测试、API 测试、性能测试、还是自动化测试？"
    │
    └─ 如果用户无法描述：
        └─→ bmad-qa（让 QA 核心帮你判断）
```

---

## 🔗 任务组合流

### 组合 1: 完整 Web 测试

```
"测试 Web 页面"
    │
    ├─ playwright-cli（快速调试）
    │     → 截图定位问题
    │
    └─ webapp-testing（完整 E2E）
          → 完整交互测试
          → 断言验证
```

### 组合 2: API 验证 + Mock

```
"验证 API 并测试"
    │
    ├─ api-documenter（验证文档）
    │     → OpenAPI 规范
    │
    └─ api-mock（Mock 测试）
          → 创建 Mock
          → 场景测试
```

### 组合 3: 性能 + 自动化

```
"做性能测试并自动化"
    │
    ├─ performance-engineer（性能）
    │     → 负载测试
    │     → Core Web Vitals
    │
    └─ test-automator（自动化）
          → 性能回归测试
          → CI/CD 集成
```

---

## 🔗 与 gql-qa 主 skill 联动

**注意**：`qa-ext-skill` 不会覆盖 `gql-qa` 主 skill，它们协同工作。

```
┌─────────────────────────────────────────────────────────────┐
│  gql-qa 主 skill                                           │
│    │                                                        │
│    ├─ 通用 QA 任务 → qa-ext-skill（路由）                  │
│    │              └─→ 具体 skill 执行                         │
│    │                                                        │
│    └─ 特定技能任务 → 直接调用具体 skill                       │
└─────────────────────────────────────────────────────────────┘
```

**何时使用 qa-ext-skill**：
- 任务模糊，需要判断用哪个 skill
- 复杂任务需要多 skill 组合
- 不确定某个 skill 是否适用

**何时直接调用具体 skill**：
- 任务明确，比如"测试 contact 区块"
- 已确定需要哪个 skill
- 只需要单个 skill

---

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 说明 |
|-------|-------|------|
| bmad-qa.md | QA 评审核心 | Sprint、缺陷报告 |
| webapp-testing.md | Playwright E2E | 页面测试 |
| verification-before-completion.md | QA 铁律 | 验证优先 |
| playwright-cli.md | CLI 工具 | 截图、DOM |
| test-automator.md | 测试自动化 | TDD/BDD |
| performance-engineer.md | 性能测试 | k6、Core Web Vitals |
| api-documenter.md | API 文档 | OpenAPI |
| test-runner.md | 自动测试 | 事件触发 |
| api-mock.md | Mock 服务器 | FastAPI |
| run-tests-after-changes.md | Git Hooks | 编辑后测试 |

---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说"测试" | 说明测试什么（Web？API？性能？） |
| 不验证就 claim 完成 | 先 `verification-before-completion` |
| Web 测试前不等待 networkidle | `page.wait_for_load_state('networkidle')` |
| API 测试不验证 OpenAPI | 用 `api-documenter` 先验证文档 |
| 忘记 Fallback | 无法匹配时 → bmad-qa |

---

## 🔗 相关角色联动

| 角色 | 协作场景 |
|------|---------|
| **coder** | 代码开发：coder-ext-skill 写代码 → qa-ext-skill 验证 |
| **review** | 安全测试：review-ext-skill 做安全审计 → qa-ext-skill 验证 |
| **arc** | 架构评审：arc-ext-skill 设计 → qa-ext-skill 评审可测试性 |
| **sm** | Sprint 规划：sm-ext-skill 规划 → qa-ext-skill 评审测试策略 |

---

## 🗣️ 示例对话

### 示例 1: 路由到 webapp-testing

```
用户：测试 contact 区块
AI：分析：包含"contact"、"区块"
     路由到 webapp-testing
     执行：Playwright E2E 测试
```

### 示例 2: 路由到 api-mock

```
用户：Mock 一个登录 API
AI：分析：包含"Mock"、"登录"
     路由到 api-mock
     执行：FastAPI Mock 服务器
```

### 示例 3: Fallback 处理

```
用户：帮我测试一下
AI：分析：包含"测试"
     但无法确定具体类型
     Fallback → bmad-qa
     执行：QA 核心判断任务类型
     反馈：这个任务是"Web E2E 测试"，建议用 webapp-testing
```

---

## 升级说明

查看 [update_readme.md](update_readme.md) 了解如何同步最新 skill。

当前版本：v2.0.0
