# 架构说明

## 项目结构

Northstar Ops 是一个前后端分离的工单协作系统示例。`ARCHITECTURE.md` 记录项目结构、Cairn 文件职责、信息流、代码组织、依赖方向、新代码归属和启动顺序；业务目标、验收范围、接口参数和代理执行规程不写在这里。

### Cairn 文件职责

```text
AGENTS.md          代理启动、读取和同步协议
.cairn/plan.md     项目定位、功能范围、数据模型、验收标准
.cairn/ARCHITECTURE.md 项目结构、信息流、代码组织和依赖方向
.cairn/TODO.md     当前执行步骤、子项、阻塞和验证结果
.cairn/fix_*       审查、测试反馈和实现偏差闭环
.cairn/fix-plan_*  plan 范围新增或修订闭环
.cairn/HUMAN.md    人工执行事项和方向性人工决策
.cairn/INTERFACE.md 对外接口契约
.cairn/TEST.md     测试方法索引
.cairn/NICKNAME.md 项目术语和别名
.cairn/archive/    已闭环历史
```

### 信息流

```text
.cairn/plan.md -> .cairn/TODO.md -> 实现与验证 -> .cairn/archive/
.cairn/fix_*.md -> .cairn/TODO.md -> 修复闭环 -> .cairn/plan.md 按需修正 -> .cairn/archive/
.cairn/fix-plan_*.md -> .cairn/plan.md -> .cairn/TODO.md -> .cairn/archive/
.cairn/TODO.md -> .cairn/HUMAN.md -> 人工反馈 -> .cairn/TODO.md
.cairn/INTERFACE.md / .cairn/TEST.md / .cairn/NICKNAME.md 按触发条件同步
```

## 代码组织

```text
apps/
  web/              浏览器端工作台、列表、详情、批量操作和审计页
  api/              HTTP API、认证入口、权限校验、工单写入
  worker/           通知、SLA 扫描、导出、重试等异步任务
packages/
  domain/           工单、备注、分派、审计事件等核心领域模型
  permissions/      角色、能力、团队范围和授权判断
  contracts/        API 请求 / 响应类型、错误结构、事件 payload
  config/           环境变量解析和共享配置
db/
  migrations/       数据库迁移
  seeds/            本地和测试环境种子数据
```

## 新代码归属

| 新增内容 | 放置位置 |
| --- | --- |
| 页面、表单、列表状态、浏览器交互 | `apps/web` |
| HTTP 路由、请求校验、响应组装 | `apps/api` |
| 异步任务、队列消费者、重试逻辑 | `apps/worker` |
| 工单状态、备注、分派、审计等领域规则 | `packages/domain` |
| 权限能力、角色范围、授权判断 | `packages/permissions` |
| 前后端共享类型、错误码、事件 payload | `packages/contracts` |
| 环境配置、启动配置、共享常量 | `packages/config` |
| 表结构变化 | `db/migrations` |

找不到明确归属的新代码，先更新本文件，再实现。

## 依赖方向

```text
apps/web    -> packages/contracts
apps/api    -> packages/domain -> packages/permissions
apps/api    -> packages/contracts
apps/worker -> packages/domain -> packages/contracts
apps/*      -> packages/config
```

- `apps/web` 不直接依赖 `packages/domain` 或 `packages/permissions`。
- `packages/domain` 不依赖 `apps/*`。
- `packages/contracts` 不依赖应用层代码。
- `apps/worker` 不承接必须同步返回给用户的请求。

## 启动顺序

本地开发按以下顺序启动：

1. 加载 `packages/config` 中的环境配置。
2. 启动数据库并执行 `db/migrations`。
3. 启动 `apps/api`，暴露 HTTP 接口。
4. 启动 `apps/worker`，连接队列和后台任务。
5. 启动 `apps/web`，通过 API 访问服务。

## 扩展点

- 新增对外接口：先在 `packages/contracts` 定义契约，再在 `apps/api` 实现路由，并同步 `.cairn/INTERFACE.md`。
- 新增异步流程：先在 `packages/contracts` 定义事件 payload，再在 `apps/worker` 增加消费者。
- 新增权限能力：优先放入 `packages/permissions`，由 `apps/api` 调用；前端只消费 API 返回的能力结果。
- 新增数据表：迁移写入 `db/migrations`，相关领域对象写入 `packages/domain`。
