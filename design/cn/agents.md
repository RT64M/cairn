# AGENTS.md 设计

`AGENTS.md` 是代理协作协议本身。它不面向最终用户，而是告诉代理进入项目后应该读什么、缺什么补什么、什么情况下能改哪些文件。

## 为什么需要

没有 `AGENTS.md` 时，每个代理都会重新猜项目规则：先读架构还是 TODO、测试失败写哪里、人工事项怎么交接、接口变更是否要改文档。`AGENTS.md` 把这些判断固定成规则。

## 应写内容

- 进入项目时的启动和读取协议。
- `TODO.md` 状态符号。
- `fix_<desc>.md` 与 `fix-plan_<desc>.md` 生命周期，包括 plan 来源标注和 fix-plan 优先归档顺序。
- `HUMAN.md` 分流规则。
- `ARCHITECTURE.md` / `INTERFACE.md` / `TEST.md` / `NICKNAME.md` 的触发和同步规则。
- 状态同步、注释等跨项目执行约束。

## 不写内容

- 用户安装教程。
- 功能宣传和 FAQ。
- 具体业务计划。
- 项目结构、文件职责、信息流和代码组织。
- 当前任务进度。
- 某次 review 的问题清单。

这些分别属于项目用户文档、`plan.md`、`ARCHITECTURE.md`、`TODO.md`、`fix_<desc>.md` 或 `fix-plan_<desc>.md`。

## 模板

- 中文模板：[../../template/AGENTS.zh.md](../../template/AGENTS.zh.md)
- English template: [../../template/AGENTS.en.md](../../template/AGENTS.en.md)

下游项目通常把其中一份复制为项目根目录的 `AGENTS.md`，再在末尾追加项目特有约定。
