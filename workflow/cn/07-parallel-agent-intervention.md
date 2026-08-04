# 工作流 7：旁路 agent 介入（不打断主 agent）

> 场景：主 agent 正在持续推进某个 TODO，用户临时让另一个 agent 做审查、补充资料、修正文档或提出计划修订；旁路工作需要被主线吸收，但不应打断主 agent 当前上下文。

## 角色分工

- **主 agent**：继续推进当前 `.cairn/TODO.md` 中的执行步骤，在自然同步点检查 Cairn 文件变更。
- **旁路 agent**：只处理被临时指派的补充任务，把结果写入对应元文件，不直接抢占主 agent 的工作上下文。
- **用户**：可以随时发起旁路审查、资料补充、人工反馈或计划讨论，并通过文件让主线在同步点接住结果。

## 典型用法

1. 主 agent 正在实现 `TODO.md` 中的某个步骤，例如 `步骤 12: 批量分派队列化`。
2. 用户不想中断主 agent，于是让另一个 agent 做旁路工作，例如：
   - 审查当前实现是否偏离 plan。
   - 补充某个接口或测试说明。
   - 根据新想法草拟 plan 修订。
   - 检查文档表述是否和当前行为一致。
3. 旁路 agent 读取必要上下文后，只把结论写入合适的 Cairn 文件。
4. 主 agent 在下一次同步点按默认读取顺序读取结构、未归档 `fix_*`、`fix-plan_*`、`.cairn/HUMAN.md`、`.cairn/TODO.md` 以及按需补充文件变更。
5. 主 agent 按协议优先级吸收旁路结果，再继续推进主线。

## 旁路结果写入哪里

| 旁路工作类型 | 写入位置 | 主 agent 后续动作 |
| --- | --- | --- |
| 发现已完成内容的 bug、实现偏差、review 问题 | `.cairn/fix_<desc>.md` | 先映射到 `TODO.md`，按 fix 优先级处理 |
| 提出 plan 之外的新功能、新模块、验收标准扩展 | `.cairn/fix-plan_<desc>.md` | 走讨论、最终确认、写入 `plan.md`、归档 fix-plan、再拆 TODO |
| 需要真实环境操作、人类权限、方向性决策 | `.cairn/HUMAN.md` | 暂停受影响范围，继续推进不依赖该决策的 TODO |
| 对外接口签名、错误码、调用示例补充 | `.cairn/INTERFACE.md` | 若接口变化影响当前 TODO，在 TODO 中注明已同步 |
| 用户要求整理测试方法或回归矩阵 | `.cairn/TEST.md` | 后续测试发现的问题仍进入 `fix_<desc>.md` |
| 新增项目黑话、别名、缩写 | `.cairn/NICKNAME.md` | 后续代理进入项目时在 `AGENTS.md` 后优先读术语表 |
| 仅补充已存在 TODO 步骤的子任务、状态或验收说明 | `.cairn/TODO.md` | 主 agent 在同步点合并执行台账；不得把旁路请求本身作为新 TODO 来源 |

## 同步点

主 agent 不需要每秒轮询旁路结果，但在以下时机必须重新检查：

- 每次会话开始。
- 完成一个 TODO 子项之后。
- 准备修改 `.cairn/plan.md`、`.cairn/TODO.md`、`.cairn/INTERFACE.md`、`.cairn/TEST.md` 等元文件之前。
- 用户明确提示“另一个 agent 已经写了反馈 / 补充 / 修订”之后。
- 准备宣称当前任务完成之前。

同步时按默认读取顺序处理：根目录 `AGENTS.md` → `.cairn/NICKNAME.md`（若存在）→ `.cairn/ARCHITECTURE.md` → 未归档 `.cairn/fix_*` → 未归档 `.cairn/fix-plan_*` → `.cairn/HUMAN.md` → `.cairn/TODO.md` → `.cairn/plan.md`；`.cairn/INTERFACE.md` / `.cairn/TEST.md` 按触发条件读取。

## 示例

主 agent 正在做：

```text
.cairn/TODO.md
- [ ] 12.3 实现批量分派 worker 的失败重试
```

用户同时让旁路 agent 检查接口契约。旁路 agent 发现 `POST /tickets/bulk-assign` 的错误码文档缺少 `409_CONFLICTING_STATUS`，但实现已经返回该错误码。

旁路 agent 不打断主 agent，也不直接改主 agent 正在写的 worker 文件，而是更新：

```text
.cairn/INTERFACE.md
- 补充 POST /tickets/bulk-assign 的 409_CONFLICTING_STATUS 错误码说明。

.cairn/TODO.md
- [ ] 12.4 同步批量分派接口错误码说明（来源：plan.md 当前版本目标；旁路 agent 已同步 INTERFACE.md）
```

主 agent 完成 `12.3` 后同步 TODO，看到 `12.4`，确认 `INTERFACE.md` 已更新，再根据需要补测试或直接标记该子项完成。

如果旁路 agent 发现的是实现 bug，则应创建 `fix_bulk-assign-contract.md`，而不是只改 `TODO.md`；主 agent 下一次同步时按 fix 优先级先处理。

## 反模式

- 旁路 agent 直接改 `plan.md`，绕过 `fix-plan_<desc>.md` 和用户最终确认。
- 旁路 agent 直接改主 agent 正在编辑的同一段代码，导致上下文互相覆盖。
- 旁路 agent 只把反馈留在聊天里，不写入 Cairn 文件，主 agent 无法感知。
- 旁路 agent 把旁路请求、当前会话或自己的一句话结论作为新 TODO 来源，而不是回到 `plan.md` / `fix_*` / `fix-plan_*`。
- 把缺陷反馈和计划扩展混进同一个 `fix_<desc>.md`。
- 主 agent 宣称完成前不重新检查旁路写入的未归档 `fix_*`、`fix-plan_*` 或 `TODO.md` 变更。
