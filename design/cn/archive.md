# .cairn/archive/ 设计

`.cairn/archive/` 保存已经收敛的 TODO 步骤、fix 批次、fix-plan 批次和人工干预历史。

## 为什么需要

活跃文件不能无限膨胀，但历史也不能被删除。归档让当前状态保持清晰，同时保留 `[~]` 废除项、`[!]` 修改路线和反馈闭环记录。

## 归档类型

| 文件 | 来源 |
| --- | --- |
| `.cairn/archive/done-YYYYMMDD.md` | 已完成或废除收敛的 TODO 步骤 |
| `.cairn/archive/fix_desc-YYYYMMDD.md` | 已清空并回写 plan 影响的 fix 批次 |
| `.cairn/archive/fix-plan_desc-YYYYMMDD.md` | 已写入 plan、完成来源与日期标注的 fix-plan 批次 |
| `.cairn/archive/human-YYYYMMDD.md` | 已完成或关闭的人工执行、人工决策或方向风险详情 |

## 规则

- 归档不删除内容，只移动位置。
- 归档标题保留原标题并注明归档日期。
- TODO 步骤满足归档条件时必须在同次会话内归档；归档是完成闭环的一部分，不是可选清理。
- `fix-plan` 默认在完成 `plan.md` 修改和来源、日期标注后立即归档，不等待后续 TODO 实现完成。
- 后续 TODO 只记录从新版 plan 派生出的执行状态，不反向阻塞已完成的 fix-plan 归档。
- 同一天多批 TODO 归档可用 `-2.md`、`-3.md`。
- TODO 原位留下可追溯面包屑。
- 仍有 `[ ]`、`[!]` 或阻塞的步骤不得归档。
