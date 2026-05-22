# QA Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/version-2.0.0-green.svg)](SKILL.md)
[![Skills](https://img.shields.io/badge/skills-10-orange.svg)](references/)
[![Category](https://img.shields.io/badge/category-gql--bots-blue.svg)](#)
[![Auto-Route](https://img.shields.io/badge/auto--route-true-blue.svg)](#)

> QA 角色智能技能路由器 - 接收任何 QA 任务，智能推荐最合适的 skill

## 核心定位

**QA 角色的中央路由器**。任何 QA 任务进来，先查这里，再路由到具体 skill。

## 一句话路由规则

```
任务包含...         → 直接路由到...
────────────────────────────────────────────────────────────
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

## 快速开始

### 安装

```bash
git clone https://github.com/relunctance/qa-ext-skill.git
```

### 使用

```bash
# Web 测试
hermes -p qa -s webapp-testing -q "测试 contact 区块"

# 浏览器调试
hermes -p qa -s playwright-cli -q "页面打不开帮我看"

# API 文档验证
hermes -p qa -s api-documenter -q "验证 OpenAPI 文档"

# 性能测试
hermes -p qa -s performance-engineer -q "执行负载测试"

# 完成前验证
hermes -p qa -s verification-before-completion -q "确保所有测试通过"
```

## 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| **bmad-qa** | QA 评审核心，处理 Sprint 测试评审、缺陷报告、测试报告 | P0 | 评审、Sprint、计划 |
| **webapp-testing** | Playwright Python E2E 测试，服务器生命周期管理 | P0 | Web、E2E、页面测试 |
| **verification-before-completion** | QA 铁律：完成前必须验证，证据优先 | P0 | 完成、通过、验证 |
| **playwright-cli** | CLI 工具，快捷浏览器操作、截图、DOM 侦查 | P0 | 打不开、页面、截图 |
| **test-automator** | 测试自动化规划，TDD/BDD，AI 测试 | P0 | 自动化、写测试、TDD |
| **performance-engineer** | 性能测试，k6/JMeter，Core Web Vitals | P1 | 性能、负载、压测 |
| **api-documenter** | OpenAPI 文档验证，SDK 生成 | P1 | API、接口、文档 |
| **test-runner** | 代码修改后自动运行相关测试 | P1 | 代码改、commit |
| **api-mock** | FastAPI Mock 服务器，场景管理 | P2 | Mock、模拟、stub |
| **run-tests-after-changes** | Git Hooks 触发测试 | P2 | 代码改、编辑后测试 |

## 智能路由决策树

```
收到 QA 任务
    │
    ├─ 包含 "Web" / "E2E" / "页面" / "contact"
    │   └─→ webapp-testing + playwright-cli
    │
    ├─ 包含 "API" / "接口" / "OpenAPI"
    │   └─→ api-documenter + api-mock
    │
    ├─ 包含 "性能" / "负载" / "压测" / "LCP"
    │   └─→ performance-engineer
    │
    ├─ 包含 "自动化" / "写测试" / "TDD"
    │   └─→ test-automator
    │
    ├─ 包含 "完成" / "通过" / "验证" / "确保"
    │   └─→ verification-before-completion
    │
    ├─ 包含 "代码改" / "commit" / "push"
    │   └─→ test-runner + run-tests-after-changes
    │
    └─ 包含 "评审" / "Sprint" / "测试计划"
        └─→ bmad-qa
```

## References TL;DR 索引

| 文件 | TL;DR | 行数 |
|------|-------|------|
| `references/bmad-qa.md` | QA评审核心：Sprint测试评审、缺陷报告、测试报告模板 | 526 |
| `references/webapp-testing.md` | Playwright Python E2E：服务器管理、networkidle等待 | 95 |
| `references/verification-before-completion.md` | QA铁律：完成前必须验证、证据优先 | 139 |
| `references/playwright-cli.md` | CLI工具：open/goto/snapshot/click/route/screenshot | 299 |
| `references/test-automator.md` | 测试自动化：TDD红绿重构、AI自愈测试 | 203 |
| `references/performance-engineer.md` | 性能测试：k6/JMeter、Core Web Vitals | 150 |
| `references/api-documenter.md` | API文档：OpenAPI 3.1+验证、SDK生成 | 161 |
| `references/test-runner.md` | 自动测试：代码修改后触发 | 28 |
| `references/api-mock.md` | Mock服务器：FastAPI动态路由、场景管理 | 1335 |
| `references/run-tests-after-changes.md` | Git Hooks：编辑后自动测试 | 28 |

## 目录结构

```
qa-ext-skill/
├── SKILL.md                   # 智能路由器（核心）
├── README.md                  # 本文件
├── LICENSE                   # MIT License
├── qa-ext-skill-执行计划.md   # 详细执行计划
├── update_readme.md          # 升级方案
├── learns/                   # 踩坑记录
│   └── README.md
└── references/               # Skill 文件副本（含TL;DR）
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

## 升级

详见 [update_readme.md](update_readme.md)

详细执行计划见 [qa-ext-skill-执行计划.md](qa-ext-skill-执行计划.md)

## 作者

- Author: relunctance
- Organization: GQL-BOT Team
