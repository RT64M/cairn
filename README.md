<a id="readme-en"></a>

# Cairn — A file-based protocol for humans + AI agents

> **Your agents forget. Your team forgets. Files don't.**
> Stack files like trail markers — and any agent finds the trail.

**English** | [中文](#readme-cn)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## ✦ What is this

**Cairn is a Markdown-based collaboration protocol.** Copy `AGENTS.md` into your project root and it takes effect — no installation, no build, no runtime dependency.

> *A cairn ([kern]) is a stack of stones hikers leave along a trail.* In this project, each "stone" is a Markdown file; together they mark the path of your project, so any agent or new teammate can follow the trail without getting lost.

Drop it into your repo root, and AI agents plus human collaborators all share the same **traceable context**: what's being worked on, why, who is blocked, what must be done or decided by a human, and how each change feeds back into the plan. Cairn keeps its own working state in `.cairn/`; the project root keeps only `AGENTS.md` as the Cairn entry file.

In short: **let memory live in files, not in chat windows.**

## ✦ The problem it solves

If you've ever used AI agents on a real project, these scenes will feel familiar:

- 🧠 **Agent amnesia / plan drift.** New session, new agent — and you're explaining the project from scratch again. Last week's design tradeoffs are buried in chat history; a few weeks in, nobody remembers what the project was supposed to be.
- 🤝 **Multi-agent collisions.** You let Codex take a pass, then Claude Code picks it up. They disagree on what "done" means and quietly overwrite each other's work.
- 🔁 **External agents interrupt the main line.** You want to ask another agent to review direction, revise content, or suggest changes at any time, but the feedback often stays in chat and the main agent cannot tell that TODO, fix, or plan boundaries changed.
- 👤 **Lost human tasks and decisions.** When an agent hits something it cannot independently verify, solve, or responsibly decide, execution either silently blocks or the issue is quietly dropped — never handed back to a human, never logged in any ledger.

Cairn gives each kind of information a **dedicated home with strict boundaries**.

This is especially useful when a main agent is already deep in continuous work: sidecar agents can review, supplement context, add interface notes, report test feedback, or propose plan revisions in parallel. Their output lands in files, and the main agent merges it at sync points without losing its current context or dropping sidecar input.

## ✦ The 30-second mental model

```text
                  ┌──────────────────────────────────────────┐
                  │       Human  ⇄  Multiple AI Agents       │
                  └─┬─────────────────────────────────────┬──┘
       feedback /   │                                     │  writes code /
       reviews /    ▼                                     ▼  progresses tasks
       tests

   ┌────────────────────┐  items enter    ┌──────────────┐  agent can't do   ┌─────────────┐
   │ fix_<desc>.md      │ ──────────────▶ │   TODO.md    │ ────────────────▶ │  HUMAN.md   │
   │ fix-plan_<desc>.md │                 │ work ledger  │  or needs human   │ intervention│
   │ (semantic name)    │                 │  (exec hub)  │ ◀──────────────── │             │
   └─────────┬──────────┘                 └──────┬───────┘   done: [x] / [~] └─────────────┘
             │ the ONLY two ways                 ▲
             │ to rewrite plan.md:               │ initial breakdown
             │ fix on archive (corrections)      │ into steps
             │ fix-plan on confirmation          │
             │ (adds/revises + annotates)        │
             ▼                                   │
   ┌────────────────┐                            │
   │    plan.md     │ ───────────────────────────┘
   │ project outline│
   └────────────────┘

── Foundation: read in order on every project entry ──────────────────────────
   AGENTS.md        bootstrap/read protocol behind every arrow above
   NICKNAME.md      project glossary and naming preferences; read early
                    (built once ≥5 terms exist)
   ARCHITECTURE.md  project structure, information flow, code ownership

── Conditional: created only when triggered ──────────────────────────────────
   INTERFACE.md     external API/CLI/SDK contract; synced with code,
                    noted as "INTERFACE.md synced" inside TODO entries
   TEST.md          test methods (only when user explicitly requests testing);
                    bugs found via TEST flow back into fix_<desc>.md

── Terminus: archive ────────────────────────────────────────────────────────
   archive/         archived fix batches · finished TODO steps · done HUMAN

All files in this diagram except root `AGENTS.md` live under `.cairn/`.
```

- **Active loop:** `fix / fix-plan → TODO ⇄ HUMAN`. `fix` archives correct plan descriptions on demand; after user final confirmation, `fix-plan` writes and annotates `plan.md`, archives first, then TODO is derived from the revised plan.
- Stable foundation and conditional supplements each own one concern; closed batches move to `.cairn/archive/` so history is preserved without cluttering the active set.

## ✦ What the file set buys you

Everything above is machinery. These are the two payoffs it exists for.

### A new session picks the project up in one read

Switch agents mid-project, open a fresh session a month later, or hand the repo to someone new — ramp-up is a fixed, bounded read:

`AGENTS.md` → `NICKNAME.md` → `ARCHITECTURE.md` → open `fix_*` → open `fix-plan_*` → `HUMAN.md` → `TODO.md` → `plan.md`

That order answers, in sequence: how we work here → what the local vocabulary means → how the code is laid out → what feedback is still open → what the plan is being changed into → what is waiting on a human → what is in flight → what the project is supposed to become. An agent that has read those files has effectively been handed over to.

The set is bounded *by design*: 4 required files plus at most 6 conditional types, with closed batches moved out to `.cairn/archive/`. It stays loadable in one pass no matter how many months the project runs — no scrolling chat history, no reading 300 commits.

### Any change traces back to the thinking behind it

The constraints exist so this chain is always walkable, from any end:

```text
a line of code
  └─ TODO step ──(source field)──▶ plan.md item  or  fix_<desc>.md batch
        │                              └─(source marker + date)──▶ .cairn/archive/fix-plan_<desc>-YYYYMMDD.md
        │                                                              └─ the discussion, the alternatives, the confirmation
        └─(on completion)──▶ .cairn/archive/done-YYYYMMDD.md
                                └─ how it was actually executed and verified
```

- **From a plan item → why it exists.** Every added or revised item in `plan.md` carries exactly one source marker with dates. Follow it into `.cairn/archive/` and you land on the batch that produced it: the discussion scope, what the agent proposed, and what the user actually approved.
- **From a change → why it was done that way.** Its TODO step names its source; that source is a plan item or a `fix_<desc>.md` batch; every batch header records content, time, source, and scope.
- **From an abandoned approach → why it was dropped.** Rejected work is never deleted. It stays as `[!] modified: <reason>` or `[~] removed: <reason / target item>` — precisely the thing git can never show you, because there was no commit.

Ordinary tooling gives you the first hop and stops. A commit message tells you *what* changed; it rarely tells you which discussion authorized the change, or which alternative was rejected getting there.

## ✦ The 60-second quickstart

```bash
# Copy the AGENTS template into your project
cp template/AGENTS.en.md your-project/AGENTS.md
cd your-project
```

Open any AI agent that can read your repo and just say:

> Read AGENTS.md and bootstrap the required meta-files for this project per its rules.

When your agent supports **Plan mode**, use it for the initial `.cairn/plan.md` creation. Cairn expects the planning stage to be interactive: discuss positioning, feature scope, data model, interfaces, and acceptance criteria first, then write the plan only after final user confirmation. Plan mode fits that “ask first, write after confirmation” initialization flow.

If you ask an agent to **create a brand-new project directly**, make sure `AGENTS.md` exists before any other project files. The safest path is to place this `AGENTS.md` in the target project folder first. If the agent is also creating the folder, add a persistent memory or global rule for that agent: before creating any new project, create the project folder and immediately write Cairn's `AGENTS.md` into it, then create `.cairn/` for the remaining Cairn files.

The agent will create `.cairn/plan.md` / `.cairn/ARCHITECTURE.md` / `.cairn/TODO.md`, and `.cairn/INTERFACE.md` / `.cairn/NICKNAME.md` / `.cairn/HUMAN.md` if their trigger conditions are met.

## ✦ Core: 4 required protocol files + up to 6 conditional supplementary file types

Every Cairn project follows the same protocol layout. **Responsibilities never overlap. Each piece of Cairn-owned information has exactly one home.**

### Required protocol files (4, mandatory in every Cairn project)

| File | Role | Change rate |
| --- | --- | --- |
| **`.cairn/plan.md`** | Outline: positioning / feature list / data model / interfaces / acceptance | Initialization only (rewritten only on fix archive or after fix-plan confirmation) |
| **`.cairn/ARCHITECTURE.md`** | Project structure: file ownership / information flow / code organization / startup order | Initialization only |
| **`AGENTS.md`** | Agent collaboration protocol: bootstrap/read protocol / status conventions / sync rules | Low frequency |
| **`.cairn/TODO.md`** | Work ledger: steps / sub-items / blockers / source / status | Per-session sync |

### Conditional supplementary files (up to 6 types, created on trigger)

| File | Trigger | Note |
| --- | --- | --- |
| **`.cairn/fix_<desc>.md`** | User requests review / audit / plan re-check, or provides test feedback | Defect-feedback loop, semantic naming; rewrites `plan.md` description on archive only when current state has diverged |
| **`.cairn/fix-plan_<desc>.md`** | User proposes a new feature / new module / acceptance scope extension outside the current plan, or asks to revise existing plan items | Plan-revision loop, semantic naming; **writes into `plan.md`, annotates source, and archives first after user final confirmation — the only entry for adding / revising plan content** |
| **`.cairn/HUMAN.md`** | A human-only task appears, or a direction-level question needs human decision | The agent packages work it cannot complete or should not decide alone; humans can edit status and feedback any time while the agent pauses the affected scope and continues other work |
| **`.cairn/INTERFACE.md`** | The project exposes any external interface (API / CLI / SDK / FE↔BE) | — |
| **`.cairn/TEST.md`** | The project is complex and the user explicitly asks the agent to test | — |
| **`.cairn/NICKNAME.md`** | Project-specific terminology reaches 5 or more terms | Captures your naming preferences so agents understand project slang, abbreviations, and aliases first |

Full rules: [`template/AGENTS.en.md`](template/AGENTS.en.md).

## ✦ A few opinionated rules

The following are Cairn's core hard constraints. Each one maps directly to a specific collaboration failure mode:

- **`plan.md` is rewritten only via two entry points:** `fix_<desc>.md` corrects description divergence on archive; `fix-plan_<desc>.md` writes new / revised content after user final confirmation, with source and date annotations. Prevents the outline from drifting in every session.
- **`fix` and `fix-plan` are kept strictly separate.** Even if the user raises a bug fix and a new feature in the same conversation, two distinct files must be created — the prefix tells you the batch's nature.
- **`fix` files use semantic names** (`fix_gesture-scoring.md`, not `fix_01.md`). The filename alone tells you the batch's subject.
- **All open `fix` and `fix-plan` files outrank `TODO.md`.** Defect feedback closes first; plan changes complete the `plan.md` write, source annotation, and fix-plan archive before TODO continues, so external input never gets dropped.
- **Cairn owns its folder.** Root `AGENTS.md` remains visible; all other protocol files live in `.cairn/`, so Cairn can manage its state without cluttering or competing with project source files.
- **Sidecar agents can work without interrupting the main agent.** The main agent can keep progressing the current TODO while other agents review, supplement context, revise docs, or propose plan changes in parallel. Sidecar results land in `fix_*`, `fix-plan_*`, `HUMAN.md`, `TODO.md`, or conditional meta-files, and the main agent merges them at sync points without losing context or leaving feedback trapped in chat.
- **What an agent cannot do or should not decide alone must move to `HUMAN.md`.** The agent packages human-only work, key confusion, or direction risks for a human; humans can edit status, feedback, or completion results in the file at any time. The agent pauses the affected scope while continuing other executable work that does not depend on that decision.
- **Project slang belongs in `NICKNAME.md`.** It is not a generic glossary; it captures your naming preferences so agents understand abbreviations, aliases, and project-specific meanings before reading the rest.
- **Deprecated TODO items are kept with `[!]` / `[~]` status.** Historical decisions stay traceable instead of being silently deleted.
- **`AGENTS.md` is for agents.** User-facing documentation is outside the Cairn template; do not mix tutorials or product marketing into the protocol.

## ✦ Repository layout

| Directory | Contents |
| --- | --- |
| [`template/`](template/) | `AGENTS.en.md` and `AGENTS.zh.md` — the two template files |
| [`design/`](design/) | Design notes split into [`design/en/`](design/en/) and [`design/cn/`](design/cn/) |
| [`workflow/`](workflow/) | Workflow samples split into [`workflow/en/`](workflow/en/) and [`workflow/cn/`](workflow/cn/) |
| [`example/`](example/) | Fictional project-state snapshots split into [`example/en/`](example/en/) and [`example/cn/`](example/cn/) |

## ✦ Reading paths

| I want to… | Read |
| --- | --- |
| Just copy and use it | [`template/AGENTS.en.md`](template/AGENTS.en.md) |
| Understand why files are split this way | [`design/en/overview.md`](design/en/overview.md) |
| See how agents act in different scenarios | [`workflow/en/01-new-project.md`](workflow/en/01-new-project.md) … [`workflow/en/07-parallel-agent-intervention.md`](workflow/en/07-parallel-agent-intervention.md) |
| See a real-looking project mid-flight | [`example/en/AGENTS.md`](example/en/AGENTS.md), [`example/en/.cairn/TODO.md`](example/en/.cairn/TODO.md), [`example/en/.cairn/fix_audit-batch.md`](example/en/.cairn/fix_audit-batch.md) |
| Walk the traceability chain on a real example | [`example/en/.cairn/plan.md`](example/en/.cairn/plan.md) (source markers) → [`example/en/.cairn/archive/fix-plan_ticket-merge-20260506.md`](example/en/.cairn/archive/fix-plan_ticket-merge-20260506.md) (the discussion behind them) |

## ✦ FAQ

**Q: How is this different from memory-bank setups, spec-driven workflows, or rules files?**

A: They solve an adjacent but different problem. Cairn makes different tradeoffs on three axes:

| Axis | Common approach | Cairn |
| --- | --- | --- |
| Information flow | One-way: code and progress → memory files, re-read next session | Closed loop: feedback and plan revisions must write back into `plan.md`, through exactly two entry points — `fix` on archive, `fix-plan` on confirmation |
| Collaboration model | Assumes one agent, one human | Assumes concurrent agents plus a human in the loop; sidecar output has a fixed landing spot, and `HUMAN.md` is an explicit channel for handing work back to a person |
| Item lifecycle | Update overwrites; prior state disappears | Status semantics (`[x]` / `[~]` / `[!]`) and an archive terminus; abandoned decisions stay traceable instead of being silently deleted |

Put differently: those approaches help **one** agent remember in the **next** session. Cairn specifies how **several** agents share state, close feedback loops, and rewrite decisions under constraint across **several months**. They coexist fine — keep tool preferences and code style in your `CLAUDE.md` or rules file; Cairn only takes over the project-state layer.

**Q: Doesn't git already record all of this?**

A: git records changes that happened; Cairn records current intent. Where they don't overlap:

- **Loadability.** A project running for months has hundreds of commits. An agent can't read them all, and can't reconstruct "why we abandoned approach A" from a sequence of diffs. Cairn's file set is bounded and fits in context in one pass.
- **What never happened.** Rejected approaches and shelved decisions leave no trace in git — there was no commit. In Cairn they are `[!]` / `[~]`, and stay traceable.
- **Human-owned work.** Tasks an agent cannot do, or should not decide alone, have no place in git. `HUMAN.md` is that place.
- **Cadence.** Commits land after work completes; a session interrupted midway leaves nothing behind. Cairn syncs within the session.

What actually overlaps with Cairn is an issue tracker, not git — and an issue tracker lives outside the repo on its own timeline. Check out a commit from three months ago and you get that day's code beside today's issue state. Cairn's files are versioned by git alongside the code, so intent and code share one timeline: at any commit, `plan.md`, `TODO.md`, and the open `fix` files are consistent with the tree.

So Cairn doesn't replace git; it builds on it — **using version control to put project intent under version control.**

**Q: How is this different from putting an "AI instructions" section in the README?**
A: That only briefs a single agent. Cairn defines how *multiple* agents share state, close feedback loops, and rewrite decisions across *multiple* sessions.

**Q: Won't this explode into a sea of files?**
A: 4 required protocol files plus up to 6 conditional supplementary file types. Only `AGENTS.md` stays at the root; the rest live in `.cairn/`. `plan.md` / `ARCHITECTURE.md` are written once at initialization and barely change after; both `fix_*` and `fix-plan_*` move into `.cairn/archive/` once closed, so the active root stays compact.

**Q: One line of code = update TODO?**
A: Yes. It is a strict requirement of the protocol. Each sync costs one line of Markdown, and in return any agent can pick the project up months later without ramp-up.

**Q: When should I not use Cairn?**

A: Short, one-off work doesn't need it. If a task is expected to finish within a single plan cycle — no follow-up sessions, no plan revisions, nothing to hand back to a human — a rules file and a TODO list will do, and Cairn's sync cost won't buy you anything.

Cairn starts paying for itself as soon as **any one** of these holds: the work spans multiple sessions or more than a few weeks; several agents take turns on the same code; human collaborators need to receive work agents cannot complete; or the plan itself will be revised more than once. A long-running solo project with a single agent qualifies too — agent amnesia doesn't spare you just because you're working alone.

This repository itself does not use Cairn — it's a protocol document with no cross-session execution state to track.

## ✦ Non-goals

- No prescriptions about language / framework / CI / branching model / testing tools.
- Does not replace your issue tracker or ADRs — it lives alongside them.
- Not bound to any agent or IDE: Cairn only specifies file structure and read/write rules; any tool that can read a Markdown repository can use it.

---

<a id="readme-cn"></a>

## 中文

[English](#readme-en) | **中文**

> **代理会忘，团队会忘，文件不会。**
> 像登山者堆石头路标一样，把"人 + 多个 AI 代理"的上下文堆在 Markdown 文件里 —— 后来的人和代理都能沿着路标走。

### ✦ 这是什么

**Cairn 是一份纯 Markdown 形式的协作协议。** 把 `AGENTS.md` 复制进项目根目录即可生效，无需安装、构建或运行时依赖。

> *Cairn（[ker-n]）原指登山者沿途堆起的石头路标。* 在这个项目里，每一块"石头"是一个 md 文件；它们一起标出项目的来路，让任何代理或新加入的人都不会迷路。

把它放进项目根目录，AI 代理和人类协作者就会共享同一份 **可追溯的上下文**：当前在做什么、为什么这么做、谁卡在哪、哪些事必须人来做或决定、改动如何回写计划。Cairn 把自己的运行状态放在 `.cairn/`；项目根目录只保留 `AGENTS.md` 作为 Cairn 入口文件。

简言之：**让记忆从聊天框里走出来，长在文件里。**

### ✦ 它解决什么问题

如果你用 AI 代理写过真实项目，下面这些场景应该熟悉：

- 🧠 **代理失忆 / plan 漂移**：换一个会话、换一个代理就要从头解释项目；上一次的设计取舍消失在聊天记录里，几周后没人记得项目原本要做什么。
- 🤝 **多代理打架**：让 Codex 改一版，再让 Claude Code 接手；两个代理对"什么算完成"理解不同，互相覆盖对方的工作。
- 🔁 **外部 agent 介入会打断主线**：你想随时请另一个 agent 复核项目方向、补充内容或提出修订，但反馈常常散落在聊天里，主 agent 不知道 TODO、fix 或计划边界已经变了。
- 👤 **人工事项和关键决策丢失**：代理遇到无法独立验证、解决或不应擅自决定的事项时，要么执行进程被静默阻塞，要么相关问题被忽略丢弃；既没交还给人类，也没进入任何台账。

Cairn 用 **职责边界清晰的几个文件** 给每类信息一个固定家。

这尤其适合“主 agent 持续工作 + 旁路 agent 随时补充”的节奏：主 agent 不必停下来等待审查或资料补充，其他 agent 可以把 review、接口补充、测试反馈或计划修订写入对应文件；主 agent 在同步点统一合流，既保留当前工作上下文，也不会漏掉旁路输入。

### ✦ 30 秒看懂

```text
                  ┌──────────────────────────────────────────┐
                  │         人类  ⇄  多个 AI 代理            │
                  └─┬─────────────────────────────────────┬──┘
       反馈/审查/   │                                     │  写代码 /
       测试        ▼                                     ▼  推进任务

   ┌────────────────────┐ 条目进入 TODO  ┌──────────────┐ 代理改不动/    ┌─────────────┐
   │ fix_<desc>.md      │ ─────────────▶ │   TODO.md    │ ─────────────▶ │  HUMAN.md   │
   │ fix-plan_<desc>.md │                │   执行台账   │  需人决策?     │  人工干预   │
   │ (语义命名)         │                │  (执行中心)  │ ◀───────────── │             │
   └─────────┬──────────┘                └──────┬───────┘  完成: [x]/[~] └─────────────┘
             │ 修改 plan 的唯一两类入口         ▲
             │ fix 归档时按需修正描述           │ 初始拆解为步骤
             │ fix-plan 确认后新增/修订并标注   │
             ▼                                  │
   ┌────────────────┐                           │
   │    plan.md     │ ──────────────────────────┘
   │   项目大纲     │
   └────────────────┘

── 地基：进入项目时按顺序读 ─────────────────────────────────────────────────
   AGENTS.md        上面所有箭头背后的启动与读取协议
   NICKNAME.md      项目术语表与命名取向；较早读取 (≥5 个术语时建立)
   ARCHITECTURE.md  项目结构、信息流、代码归属

── 按需：满足触发条件才出现 ─────────────────────────────────────────────────
   INTERFACE.md     对外接口契约；与代码同步，TODO 注 "已同步 INTERFACE.md"
   TEST.md          测试方法 (用户主动要求测试时建立)；
                    测出的 bug 流入 fix_<desc>.md

── 终点：归档收纳 ─────────────────────────────────────────────────────────
   archive/         已归档 fix 批次 · 已完成 TODO 步骤 · 已完成 HUMAN 条目

图中除根目录 `AGENTS.md` 外，其他文件都位于 `.cairn/`。
```

- 主流程闭环：**fix / fix-plan → TODO ⇄ HUMAN**；fix 归档时按需修正 plan 描述偏差，fix-plan 在用户最终确认后立即写入并标注 plan 来源，然后优先归档，再从新版 plan 拆 TODO。
- 稳定地基与按需补充各司其职，归档进 `.cairn/archive/` 不删除历史。

### ✦ 这套文件换来什么

上面都是机制，下面是它存在的两个理由。

#### 换会话、换代理，一次读完就等于交接完毕

项目做到一半换个代理、一个月后开一个新会话、或者把仓库交给新来的人——上手成本是一段固定且有界的阅读：

`AGENTS.md` → `NICKNAME.md` → `ARCHITECTURE.md` → 未归档 `fix_*` → 未归档 `fix-plan_*` → `HUMAN.md` → `TODO.md` → `plan.md`

这个顺序依次回答：这里怎么协作 → 项目黑话什么意思 → 代码怎么摆 → 还有哪些反馈没收敛 → 计划正在被改成什么样 → 什么事卡在人类那边 → 手上正在做什么 → 这个项目最终要长成什么。读完这几个文件的代理，等于已经被交接过一遍。

而且这个集合**在设计上就是有界的**：4 个核心文件 + 最多 6 类按需文件，收敛的批次全部移出到 `.cairn/archive/`。无论项目跑了多少个月，它都能一次性读进上下文——不用翻聊天记录，也不用读 300 个 commit。

#### 任何一次改动，都能反查回当初的思考过程

那些约束存在的意义，就是让下面这条链从任意一端都走得通：

```text
一处代码改动
  └─ TODO 条目 ──(来源字段)──▶ plan.md 条目  或  fix_<desc>.md 批次
        │                          └─(来源标注 + 日期)──▶ .cairn/archive/fix-plan_<desc>-YYYYMMDD.md
        │                                                    └─ 当初的讨论范围、备选方案、用户确认结论
        └─(完成归档)──▶ .cairn/archive/done-YYYYMMDD.md
                          └─ 实际怎么执行、怎么验证的
```

- **从 plan 条目 → 查它为什么存在。** `plan.md` 里每条新增或修订的条目都带且只带一个来源标注（含日期）。顺着它进 `.cairn/archive/`，就能找到产出它的那个批次：当时讨论了什么、代理提了什么建议、用户最终确认了哪些。
- **从一处改动 → 查它为什么这么做。** 对应 TODO 条目写明来源，来源是 plan 条目或 `fix_<desc>.md` 批次，而每个批次的文件头都记录了内容、时间、来源和影响范围。
- **从一个被放弃的方案 → 查它为什么被放弃。** 被否决的工作不删除，而是留成 `[!] 修改：<原因>` 或 `[~] 移除：<原因 / 替代条目>`——这恰恰是 git 永远给不了你的东西，因为它根本没有产生 commit。

常规工具只能给你第一跳就断了。commit message 告诉你**改了什么**，但很少告诉你是哪次讨论授权了这次改动，以及路上否掉了哪个备选方案。

### ✦ 60 秒上手

```bash
# 把 AGENTS 模板拷进你的项目
cp template/AGENTS.zh.md your-project/AGENTS.md
cd your-project
```

打开任何支持读取仓库的 AI 代理，直接说：

> 读一下 AGENTS.md，按里面的规则给本项目补全协议要求的元文件。

建议在支持 **Plan mode / 计划模式** 的代理里完成初始 `.cairn/plan.md` 创建。Cairn 要求计划阶段先充分交互讨论项目定位、功能清单、数据模型、接口和验收标准，再由用户最终确认；计划模式更适合这种“先问清楚，再落文件”的初始化过程。

如果是让 agent **直接创建一个全新项目**，要确保 `AGENTS.md` 先于其他项目文件出现。最稳妥的做法是在目标项目文件夹中预先放入这份 `AGENTS.md`；如果项目文件夹也由 agent 创建，则建议在 agent 的长期记忆或全局规则里写明：创建任何新项目前，先创建项目文件夹并立刻写入 Cairn 的 `AGENTS.md`，再创建 `.cairn/` 存放其余 Cairn 文件。

代理会按协议建出 `.cairn/plan.md` / `.cairn/ARCHITECTURE.md` / `.cairn/TODO.md`，并在触发条件出现时建 `.cairn/INTERFACE.md` / `.cairn/NICKNAME.md` / `.cairn/HUMAN.md`。

### ✦ 核心：4 个核心协议文件 + 6 个按需补充文件

每个采用 Cairn 的项目都遵循同一套协议文件分工。**职责互不重叠、Cairn 管理的信息只放在它的家里。**

#### 核心协议文件（4 个，所有 Cairn 项目强制）

| 文件 | 角色 | 改动频率 |
| --- | --- | --- |
| **`.cairn/plan.md`** | 项目大纲：定位 / 功能清单 / 数据模型 / 接口 / 验收 | 仅初始化（仅 fix 归档或 fix-plan 确认后回写） |
| **`.cairn/ARCHITECTURE.md`** | 项目结构：文件职责 / 信息流 / 代码组织 / 启动顺序 | 仅初始化 |
| **`AGENTS.md`** | 代理协作协议：启动读取协议 / 状态约定 / 同步要求 | 低频 |
| **`.cairn/TODO.md`** | 执行台账：步骤 / 子项 / 阻塞 / 来源 / 完成状态 | 每次会话同步 |

#### 按需补充文件（最多 6 类，触发后创建）

| 文件 | 触发条件 | 备注 |
| --- | --- | --- |
| **`.cairn/fix_<desc>.md`** | 用户要求审查 / 审计 / 复核，或提供测试反馈 | 缺陷反馈闭环，语义命名；归档时按需修正 plan 描述偏差 |
| **`.cairn/fix-plan_<desc>.md`** | 用户提出 plan 之外的新功能 / 新模块 / 验收标准扩展，或要求修订既有 plan 条目 | 计划修订闭环，语义命名；**用户最终确认后立刻写入 plan、标注来源并优先归档，是新增 / 修订 plan 内容的唯一入口** |
| **`.cairn/HUMAN.md`** | 出现代理无法完成的人工事项，或需要人类决策的方向性问题 | 代理把自己无法完成或不应擅自决定的事项打包交给人类；人类可随时改状态介入，代理暂停受影响范围并继续推进其他工作 |
| **`.cairn/INTERFACE.md`** | 项目存在对外接口（API / CLI / SDK / 前后端） | — |
| **`.cairn/TEST.md`** | 项目复杂且用户明确要求代理测试 | — |
| **`.cairn/NICKNAME.md`** | 项目专有术语 ≥ 5 个 | 交代你的独有命名取向，让代理先理解项目黑话、缩写和别名 |

完整规则见 [`template/AGENTS.zh.md`](template/AGENTS.zh.md)。

### ✦ 几个关键设计

以下是 Cairn 协议的几条核心硬性约束。每一条都直接对应一类典型的协作失败模式：

- **plan.md 仅可通过两类入口修改**：`fix_<desc>.md` 归档时按需修正描述偏差，`fix-plan_<desc>.md` 在用户最终确认后写入新增 / 修订内容，并标注来源与日期。防止每次会话都改大纲，导致项目定位漂移。
- **fix 与 fix-plan 严格分文件。** 即使用户在同一次对话中同时提出 bug 修复与新功能，也必须分别建两份文件，从文件名前缀即可判别批次性质。
- **fix 文件用语义命名**（`fix_gesture-scoring.md` 而不是 `fix_01.md`）。从文件名就能判断批次主题。
- **TODO 优先级低于所有未归档的 fix 与 fix-plan。** 缺陷反馈先收敛；plan 修改先完成写入、来源标注和 fix-plan 归档，再继续推 TODO，避免遗漏外部输入。
- **Cairn 管理自己的文件夹。** 根目录 `AGENTS.md` 保持可见；其他协议文件都放入 `.cairn/`，避免和项目源码抢位置。
- **支持旁路 agent 不打断主 agent。** 主 agent 可以持续推进当前 TODO；其他 agent 可在旁路做审查、补充资料、修正文档或提出计划修订，并把结果写进 `fix_*`、`fix-plan_*`、`HUMAN.md`、`TODO.md` 或按需补充文件。主 agent 在同步点读取这些文件后合流，既不丢上下文，也不让反馈散落在聊天里。
- **代理改不动或不应擅自决定的事项必须转 HUMAN.md。** 代理把无法完成的人工事项、关键困惑或方向风险打包提交给人类；人类可随时通过修改文件中的状态、反馈或完成结果介入。代理暂停受影响范围，同时继续处理其他不依赖该决策的可执行事项。
- **项目黑话必须交给 NICKNAME.md。** 它不是普通词典，而是你的独有命名取向说明；代理读完根目录 `AGENTS.md` 后先读它，再进入架构和状态文件，避免误解缩写、别名和项目内特殊含义。
- **TODO 的 `[!]` / `[~]` 状态保留废弃条目。** 历史决策可回溯，而不是被悄悄删除。
- **AGENTS.md 给代理读。** 面向用户的项目说明不属于 Cairn 模板；不要把教程或宣传文案混进协议。

### ✦ 仓库结构

| 目录 | 内容 |
| --- | --- |
| [`template/`](template/) | `AGENTS.en.md` 与 `AGENTS.zh.md` 两个模板文件 |
| [`design/`](design/) | 设计说明，按 [`design/en/`](design/en/) 与 [`design/cn/`](design/cn/) 分目录存放 |
| [`workflow/`](workflow/) | 工作流样例，按 [`workflow/en/`](workflow/en/) 与 [`workflow/cn/`](workflow/cn/) 分目录存放 |
| [`example/`](example/) | 虚构项目状态快照，按 [`example/en/`](example/en/) 与 [`example/cn/`](example/cn/) 分目录存放 |

### ✦ 阅读路线

| 我想… | 直接读 |
| --- | --- |
| 复制一份立刻用 | [`template/AGENTS.zh.md`](template/AGENTS.zh.md) |
| 理解为什么这样分文件 | [`design/cn/overview.md`](design/cn/overview.md) |
| 看代理在不同场景怎么走流程 | [`workflow/cn/01-new-project.md`](workflow/cn/01-new-project.md) … [`workflow/cn/07-parallel-agent-intervention.md`](workflow/cn/07-parallel-agent-intervention.md) |
| 看真实项目跑起来长什么样 | [`example/cn/AGENTS.md`](example/cn/AGENTS.md)、[`example/cn/.cairn/TODO.md`](example/cn/.cairn/TODO.md)、[`example/cn/.cairn/fix_audit-batch.md`](example/cn/.cairn/fix_audit-batch.md) |
| 在真实样例上走一遍追溯链 | [`example/cn/.cairn/plan.md`](example/cn/.cairn/plan.md)（来源标注）→ [`example/cn/.cairn/archive/fix-plan_ticket-merge-20260506.md`](example/cn/.cairn/archive/fix-plan_ticket-merge-20260506.md)（标注背后的讨论） |

### ✦ FAQ

**Q：这跟 Memory Bank / spec 驱动开发 / 各家 rules 文件有什么区别？**

A：它们解决的是相邻但不同的问题。三个维度上 Cairn 的取舍不一样：

| 维度 | 常见做法 | Cairn |
| --- | --- | --- |
| 信息方向 | 单向：代码与进展 → 记忆文件，供下次会话读取 | 双向闭环：反馈与计划修订必须回写 `plan.md`，且只有 `fix` 归档与 `fix-plan` 确认两个入口 |
| 协作模型 | 默认单 agent + 单人 | 默认多 agent 并发 + 人在回路；旁路 agent 的产出有固定落点，`HUMAN.md` 是把事交还给人类的显式通道 |
| 条目生命周期 | 更新即覆盖，旧状态消失 | 有状态语义（`[x]` / `[~]` / `[!]`）与归档终点；废弃决策保留可追溯，不静默删除 |

换句话说：那类方案让**一个** agent 在**下一次**会话里记得住；Cairn 规定**多个** agent 在**多个月**里如何共享状态、闭环反馈、以受控方式改写决策。两者可以共存——你完全可以在 `CLAUDE.md` / rules 里继续放工具偏好和代码风格，Cairn 只接管项目状态那一层。

**Q：git 已经记录了所有变更历史，为什么还需要这些文件？**

A：git 记录已经发生的变更，Cairn 记录当前的意图。两者不重叠的地方是：

- **可加载性**：跑了几个月的项目有几百个 commit，代理读不完，也无法从 diff 序列还原"当初为什么放弃方案 A"。Cairn 的文件数量有界，可一次性读进上下文。
- **没发生的事**：被否决的方案、搁置的决策在 git 里没有痕迹——因为没有 commit。在 Cairn 里它们是 `[!]` / `[~]`，可回溯。
- **人工事项**：代理做不了或不应擅自决定的事，git 里没有位置，`HUMAN.md` 有。
- **节奏**：commit 发生在工作完成后；被中途打断的会话不会留下任何东西。Cairn 在会话内同步。

真正和 Cairn 功能重叠的是 issue tracker，而不是 git。区别在于 issue tracker 是仓库外的独立时间线：你 `checkout` 到三个月前的 commit，看到的是当时的代码配今天的 issue 状态。Cairn 的文件被 git 一起版本化，代码和意图共享同一条时间线——任意一个 commit 上，`plan.md`、`TODO.md` 和未归档的 `fix` 都与代码自洽。

所以 Cairn 不替代 git，它建立在 git 之上：**用版本控制给项目意图加上版本控制。**

**Q：这跟在 README 里写一段"AI 须知"有什么区别？**
A：那只是给单个代理的提示。Cairn 规定 *多个* 代理之间如何在 *多次* 会话间共享状态、闭环反馈、回写决策。

**Q：不会让文件多到爆炸吗？**
A：4 个核心协议文件 + 最多 6 类按需补充文件。其中只有 `AGENTS.md` 留在根目录，其余都在 `.cairn/`；`plan.md` / `ARCHITECTURE.md` 仅在初始化阶段撰写，后续基本不动；`fix_*` 与 `fix-plan_*` 完成后均归档进 `.cairn/archive/`，活跃根目录始终保持清爽。

**Q：改一行代码也要更新 TODO？**
A：是。这是协议的强制约束。每次同步只需一行 markdown，换来的是数月之后任何代理都能立即接手项目。

**Q：什么情况下不该用 Cairn？**

A：一次性的短期任务不需要。如果一个任务预计在一个 plan 周期内就能收尾——不会跨越多次会话、不需要修订计划、也没有要交还给人类的事项——那么一个 rules 文件加一份 TODO 就够了，Cairn 的同步成本换不回什么。

反过来，只要下面**任意一条**成立，Cairn 就开始回本：工作会跨越多次会话或数周以上、多个代理会轮流接手同一份代码、有人类协作者需要接收代理无法完成的事项、计划本身会被反复修订。单人 + 单代理的长期项目同样适用——「代理会失忆」这件事，一个人也躲不掉。

本仓库自身就不使用 Cairn：它是一份协议文档，没有需要跨会话追踪的执行状态。

### ✦ 不做什么

- 不提供具体语言 / 框架 / CI / Git 分支模型 / 测试工具的选择。
- 不替代 issue tracker、不替代 ADR，但可以和它们共存。
- 不绑定任何代理或 IDE：只规定文件结构和读写规则，任何能读取 Markdown 仓库的工具都可用。

## ✦ License

MIT — see [LICENSE](LICENSE).
