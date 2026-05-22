---
name: qa-ext-skill
description: QA 技能索引 - 根据任务类型智能推荐合适的 QA skill
version: 1.0.0
author: relunctance
license: MIT
category: gql-bots
tags:
  - qa
  - testing
  - skill-router
  - gql-bots
hermes:
  platforms:
    hermes: true
---

# QA Ext Skill - 技能索引

> QA 角色技能路由器，根据任务类型智能推荐合适的 skill

## 快速参考表

| 场景 | 推荐 Skill | 适用任务 |
|------|-----------|---------|
| QA 评审 | bmad-qa | Sprint 测试评审 |
| Web 测试 | webapp-testing | Web 功能验证 |
| 浏览器自动化 | playwright-cli | 快速调试/截图 |
| 完成前验证 | verification-before-completion | 任何完成声明前 |
| 测试自动化 | test-automator | 自动化测试规划 |
| 性能测试 | performance-engineer | 负载/压力测试 |
| API 文档 | api-documenter | OpenAPI 验证 |
| API Mock | api-mock | 前后端并行开发 |
| 测试运行 | test-runner | CI/CD 集成 |
| 变更测试 | run-tests-after-changes | 代码修改后验证 |

---

## 技能地图

### 场景 → Skill 映射

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

### QA 工作流推荐

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

## 智能推荐规则

### 决策树

```
任务类型判断
    │
    ├─ "Web页面/E2E" → webapp-testing + playwright-cli
    │
    ├─ "API/接口测试" → api-documenter + api-mock
    │
    ├─ "性能/负载" → performance-engineer
    │
    ├─ "自动化/AI测试" → test-automator
    │
    ├─ "完成声明/验证" → verification-before-completion
    │
    ├─ "CI/CD/测试运行" → test-runner + run-tests-after-changes
    │
    └─ "评审/规划" → bmad-qa
```

### Skill 优先级

| 级别 | Skill | 说明 |
|------|-------|------|
| P0 | bmad-qa | QA 评审核心，必须 |
| P0 | webapp-testing | Web 测试核心能力 |
| P0 | verification-before-completion | QA 铁律，避免漏测 |
| P0 | playwright-cli | 快速调试与验证 |
| P0 | test-automator | 测试自动化基础 |
| P1 | performance-engineer | 性能测试 |
| P1 | api-documenter | API 文档验证 |
| P1 | test-runner | 测试运行自动化 |
| P2 | api-mock | 前后端并行开发 |
| P2 | run-tests-after-changes | 代码变更测试 |

---

## References

所有 skill 文件位于 `references/` 目录：

| Skill | 文件 | 级别 |
|-------|------|------|
| bmad-qa | `references/bmad-qa.md` | P0 |
| webapp-testing | `references/webapp-testing.md` | P0 |
| verification-before-completion | `references/verification-before-completion.md` | P0 |
| playwright-cli | `references/playwright-cli.md` | P0 |
| test-automator | `references/test-automator.md` | P0 |
| performance-engineer | `references/performance-engineer.md` | P1 |
| api-documenter | `references/api-documenter.md` | P1 |
| test-runner | `references/test-runner.md` | P1 |
| api-mock | `references/api-mock.md` | P2 |
| run-tests-after-changes | `references/run-tests-after-changes.md` | P2 |

---

## 触发条件

- `kanban create --skill qa-ext-skill`
- QA 任务需要选择合适的 skill
- 需要了解 QA 角色全部能力时

## 使用方式

```bash
# 当接到 Web 测试任务时
hermes chat -p qa -s webapp-testing -q "测试 contact 区块"

# 当接到 API 测试任务时
hermes chat -p qa -s api-documenter -q "验证 OpenAPI 文档"

# 当需要性能测试时
hermes chat -p qa -s performance-engineer -q "执行负载测试"
```

---

## 升级说明

详见 `update_readme.md`
