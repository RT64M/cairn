# 设计总览

> `Cairn`（A file-based protocol for humans + AI agents）的核心是把开发上下文从聊天和记忆中移到文件里。
> 本目录解释每个 md 文件为什么存在、边界在哪里、如何互相约束。

## 文件分层

### 核心元文件（4 个，强制）

| 文件 | 核心问题 | 设计目的 |
| --- | --- | --- |
| `.cairn/plan.md` | 这个项目为什么存在、要做成什么 | 锁定稳定大纲，避免设计漂移 |
| `.cairn/ARCHITECTURE.md` | 项目结构如何分层、信息如何流动、代码如何组织 | 锁定文件职责、信息流和代码结构，避免随会话发散 |
| `AGENTS.md` | 代理进入项目后怎么执行协议 | 给代理一份可执行的协作和同步规则 |
| `.cairn/TODO.md` | 当前做什么、做到哪、为什么阻塞 | 成为所有执行状态的中心台账 |

### 按需补充

| 文件 | 触发条件 | 设计目的 |
| --- | --- | --- |
| `.cairn/fix_<desc>.md` | 审查、测试反馈或计划复核 | 让缺陷反馈有独立生命周期，并在归档时按需修正 plan 描述偏差 |
| `.cairn/fix-plan_<desc>.md` | 新功能、新模块、验收范围或既有 plan 条目修订 | 让 plan 修改先经确认，再写入 `plan.md`、标注来源与日期、优先归档 |
| `.cairn/HUMAN.md` | 代理无法完成，或需要人类决策避免方向漂移 | 把人工执行、关键困惑和方向风险从 fix / TODO 中分流出来 |
| `.cairn/INTERFACE.md` | 项目存在对外接口 | 让接口契约与实现同步变更 |
| `.cairn/TEST.md` | 项目复杂且用户明确要求测试 | 记录测试方法，不混入 bug 反馈 |
| `.cairn/NICKNAME.md` | 项目术语达到 5 个或以上 | 让术语先于其他文件被理解 |
| `.cairn/archive/` | TODO / fix / fix-plan / HUMAN 收敛或闭环后 | 保存历史，同时让活跃文件保持清爽 |

## 信息流

```text
AGENTS.md          -> 启动协议：决定代理如何进入、同步和更新文件
.cairn/NICKNAME.md -> 若存在，先解释术语
.cairn/ARCHITECTURE.md -> 决定项目结构、文件职责、信息流和代码组织
.cairn/fix_<desc>.md -> 缺陷反馈优先进入 TODO
.cairn/fix-plan_<desc>.md -> plan 修改先确认、写入、标注来源与日期、优先归档，再拆 TODO
.cairn/HUMAN.md    -> 代理做不了或不应擅自决定的事转人工干预
.cairn/TODO.md     -> 执行中心，连接所有变更
.cairn/plan.md     -> 只通过 fix 归档或 fix-plan 最终确认后回写
.cairn/archive/    -> 保存已收敛历史
```

## 设计原则

- **单一职责**：同一信息只放在一个拥有者文件里。
- **自有工作区**：只有 `AGENTS.md` 留在项目根目录，其他 Cairn 协议文件都放在 `.cairn/`。
- **架构回收结构**：项目文件布局、信息归属、信息流和代码组织统一由 `ARCHITECTURE.md` 管理。
- **先记录再执行**：会改代码或文档的事项必须先从 `plan.md` 或 `fix_<desc>.md` 进入 `TODO.md`。
- **反馈有入口**：审查、测试、人工反馈进入语义命名的 `fix_<desc>.md`。
- **plan 修改有入口**：新增、扩展或修订 plan 内容进入 `fix-plan_<desc>.md`，最终确认后写入 `plan.md`，标注来源与日期，优先归档后再拆 TODO。
- **TODO 不收录命令本身**：用户启动命令、当前会话和旁路 agent 说明只能触发 plan / fix 流程，不能直接成为 TODO 来源。
- **外部 agent 可异步介入**：其他 agent 可随时创建 fix、fix-plan 或按需元文件；主 agent 按读取顺序识别这些变动后继续推进当前工作。
- **人工有出口**：代理无法完成或不应擅自决定的事项进入 `HUMAN.md`，受影响范围暂停，其他可执行 TODO 继续推进。
- **接口与代码同变更**：对外接口签名变化必须同步 `INTERFACE.md`。
- **测试方法和测试结果分离**：`TEST.md` 写怎么测，`fix_<desc>.md` 写测出了什么问题。

以上是「怎么分」，下面两条是「为什么值得这么分」——它们是前面所有约束共同服务的目标：

- **有界且可一次加载**：文件类型数量封顶（4 + 最多 6），收敛批次一律移出到 `.cairn/archive/`。目的是让活跃文件集不随项目时长增长，任何新会话、新代理都能一次读完全部当前状态即完成交接，而不必回溯聊天记录或 commit 历史。
- **决策全程可回溯**：`plan.md` 的单一来源标注、fix / fix-plan 的文件头四要素（内容、时间、来源、影响范围）、TODO 的来源字段、`[!]` / `[~]` 的原因说明、archive 的批次留存，共同构成一条双向可走的链：从任意一处改动能查回授权它的讨论，从任意一条 plan 条目能查到它被写入和修订的时刻。被否决的方案也在链上，因为它们不产生 commit，git 无法记录。

## 详细设计

- [AGENTS.md](agents.md)
- [plan.md](plan.md)
- [ARCHITECTURE.md](architecture.md)
- [TODO.md](todo.md)
- [fix_<desc>.md](fix.md)
- [fix-plan_<desc>.md](fix-plan.md)
- [HUMAN.md](human.md)
- [INTERFACE.md](interface.md)
- [TEST.md](test.md)
- [NICKNAME.md](nickname.md)
- [archive/](archive.md)
