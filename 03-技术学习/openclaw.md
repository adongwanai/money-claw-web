OpenClaw + Codex/ClaudeCode Agent Swarm: The One-Person Dev Team [Full Setup]  
OpenClaw + Codex/ClaudeCode Agent Swarm：单人开发团队 [完整设置]

I don't use Codex or Claude Code directly anymore.  
我不再直接使用 Codex 或 Claude Code 了。

I use OpenClaw as my orchestration layer. My orchestrator, Zoe, spawns the agents, writes their prompts, picks the right model for each task, monitors progress, and pings me on Telegram when PRs are ready to merge.  
我使用 OpenClaw 作为我的编排层。我的编排器 Zoe 会生成代理、编写它们的提示、为每个任务选择合适的模型、监控进度，并在 PR 准备好合并时通过 Telegram 通知我。

Proof points from the last 4 weeks:  
过去四周的要点：

- 94 commits in one day. My most productive day - I had 3 client calls and didn't open my editor once. The average is around 50 commits a day.  
一天之内提交了 94 次代码 。 这是我效率最高的一天——我接了 3 个客户电话，一次编辑器都没打开。平均每天提交大约 50 次代码。

- 7 PRs in 30 minutes. Idea to production are blazing fast because coding and validations are mostly automated.  
- 30 分钟内提交了 7 个 PR 。从构思到生产的速度非常快，因为编码和验证大部分都是自动化的。

- Commits → MRR: I use this for a real B2B SaaS I'm building — bundling it with founder-led sales to deliver most feature requests same-day. Speed converts leads into paying customers.  
- 订单提交 → 月度经常性收入 (MRR) ：我将其用于我正在构建的真正的 B2B SaaS 产品——结合创始人主导的销售团队，力求在当天交付大多数功能请求。速度是将潜在客户转化为付费客户的关键。

[

](https://x.com/elvissun/article/2025920521871716562/media/2025660629109895168)

before Jan: CC/codex only | after Jan: Openclaw orchestrates CC/codex  
1 月之前：仅限 CC/codex | 1 月之后：Openclaw 管理 CC/codex

My git history looks like I just hired a dev team. In reality it's just me going from managing claude code, to managing an openclaw agent that manages a fleet of other claude code and codex agents.  
我的 Git 历史记录看起来好像我刚招了个开发团队。实际上，我只是从管理 Claude 代码，转到管理一个 OpenClaw 代理，而这个代理又管理着一大批其他的 Claude 代码和 Codex 代理。

Success rate: The system one-shots almost all small to medium tasks without any intervention.  
成功率：该系统几乎可以一次性完成所有中小型任务，无需任何干预。

Cost: ~$100/month for Claude and $90/month for Codex, but you can start with $20.  
费用：Claude 每月约 100 美元，Codex 每月约 90 美元，但您可以从 20 美元开始。

Here's why this works better than using Codex or Claude Code directly:  
这就是为什么这种方法比直接使用 Codex 或 Claude Code 更有效的原因：

>Codex and Claude Code have very little context about your business.  
>Codex 和 Claude Code 对您的业务几乎没有任何参考价值。

They see code. They don't see the full picture of your business.  
他们看到的是代码，而不是你业务的全貌。

OpenClaw changes the equation. It acts as the orchestration layer between you and all agents — it holds all my business context (customer data, meeting notes, past decisions, what worked, what failed) inside my Obsidian vault, and translates historical context into precise prompts for each coding agent. The agents stay focused on code. The orchestrator stays at the high strategy level.  
OpenClaw 改变了格局。它充当您和所有代理之间的协调层——它将我所有的业务背景信息（客户数据、会议记录、过往决策、成功经验和失败教训）保存在我的 Obsidian 存储库中，并将历史背景信息转化为每个编码代理的精准提示。代理可以专注于代码编写，而协调层则可以专注于高层战略。

Here's how the system works at a high level:  
该系统的大致工作原理如下：

[

](https://x.com/elvissun/article/2025920521871716562/media/2025790010293669888)

Last week Stripe wrote about their background agent system called "Minions" — parallel coding agents backed by a centralized orchestration layer. I accidentally built the same thing but it runs locally on my Mac mini.  
上周 Stripe 介绍了他们的后台代理系统“Minions”——由集中式编排层支持的并行编码代理。我不小心也搭建了一个类似的系统，不过它在我的 Mac mini 上本地运行。

Before I tell you how to set this up, you should know WHY you need an agent orchestrator.  
在我告诉你如何设置之前，你应该知道为什么需要代理协调器。

## 

Why One AI Can't Do Both  
为什么一个人工智能不能同时做到这两件事

Context windows are zero-sum. You have to choose what goes in.  
上下文窗口是零和博弈 。你必须选择哪些内容可以显示。

Fill it with code → no room for business context. Fill it with customer history → no room for the codebase. This is why the two-tier system works: each AI is loaded with exactly what it needs.  
用代码填充 → 没有空间容纳业务上下文。用客户历史记录填充 → 没有空间容纳代码库。这就是双层系统有效的原因：每个 AI 都只加载它需要的内容。

OpenClaw and Codex have drastically different context:  
OpenClaw 和 Codex 的背景截然不同：

[](https://x.com/elvissun/article/2025920521871716562/media/2025780043406864384)

Specialization through context, not through different models.  
专业化是通过情境实现的，而不是通过不同的模型实现的。

## 

The Full 8-step Workflow  
完整的八步工作流程

Let me walk through a real example from last week.  
让我举一个上周发生的真实例子。

Step 1: Customer Request → Scoping with Zoe  
步骤 1：客户需求 → 与 Zoe 进行范围界定

I had a call with an agency customer. They wanted to reuse configurations they've already set up across the team.  
我与一家代理客户进行了通话。他们希望在团队中复用已设置的配置。

After the call, I talked through the request with Zoe. Because all my meeting notes sync automatically to my obsidian vault, zero explanation was needed on my end. We scoped out the feature together — and landed on a template system that lets them save and edit their existing configurations.  
通话结束后，我跟佐伊详细讨论了这个需求。因为我的所有会议记录都会自动同步到我的 Obsidian Vault，所以我这边不需要任何解释。我们一起探讨了这项功能，最终确定采用模板系统，让他们可以保存和编辑现有的配置。

Then Zoe does three things:  
然后佐伊做了三件事：

1. Tops up credits to unblock customer immediately — she has admin API access  
    充值即可立即解锁客户账户——她拥有管理员 API 访问权限
    
2. Pulls customer config from prod database — she has read-only prod DB access (my codex agents will never have this) to retrieve their existing setup, which gets included in the prompt  
    从生产数据库中提取客户配置 ——她拥有只读的生产数据库访问权限（我的 Codex 代理永远不会有这种权限），以检索他们现有的设置，该设置会包含在提示信息中。
    
3. Spawns a Codex agent — with a detailed prompt containing all the context  
    生成一个 Codex 代理——附带包含所有上下文的详细提示
    

Step 2: Spawn the Agent  
步骤 2：生成特工

Each agent gets its own worktree (isolated branch) and tmux session:  
每个代理都有自己的工作树（隔离分支）和 tmux 会话：

bash  
狂欢

```bash
# Create worktree + spawn agent
git worktree add ../feat-custom-templates -b feat/custom-templates origin/main
cd ../feat-custom-templates && pnpm install

tmux new-session -d -s "codex-templates" \
  -c "/Users/elvis/Documents/GitHub/medialyst-worktrees/feat-custom-templates" \
  "$HOME/.codex-agent/run-agent.sh templates gpt-5.3-codex high"
```

The agent runs in a tmux session with full terminal logging via a script.  
该代理在 tmux 会话中运行，并通过脚本进行完整的终端日志记录。

Here's how we launch agents:  
以下是我们推出代理商的方式：

bash  
狂欢

```bash
# Codex
codex --model gpt-5.3-codex \
  -c "model_reasoning_effort=high" \
  --dangerously-bypass-approvals-and-sandbox \
  "Your prompt here"

# Claude Code  
claude --model claude-opus-4.5 \
  --dangerously-skip-permissions \
  -p "Your prompt here"
```

I used to use codex exec or claude -p, but switch to tmux recently:  
我以前用过 codex exec 或 claude -p，但最近改用 tmux 了：

tmux is far better because mid-task redirection is powerful. Agent going the wrong direction? Don't kill it:  
tmux 远胜于其他方案，因为它支持任务中途重定向 。代理程序运行方向错误？别终止它：

bash  
狂欢

```bash
# Wrong approach:
tmux send-keys -t codex-templates "Stop. Focus on the API layer first, not the UI." Enter

# Needs more context:
tmux send-keys -t codex-templates "The schema is in src/types/template.ts. Use that." Enter
```

The task gets tracked in .clawdbot/active-tasks.json:  
任务跟踪记录在 .clawdbot/active-tasks.json 文件中：

json

```json
{
  "id": "feat-custom-templates",
  "tmuxSession": "codex-templates",
  "agent": "codex",
  "description": "Custom email templates for agency customer",
  "repo": "medialyst",
  "worktree": "feat-custom-templates",
  "branch": "feat/custom-templates",
  "startedAt": 1740268800000,
  "status": "running",
  "notifyOnComplete": true
}
```

When complete, it updates with PR number and checks. (More on this in step 5)  
完成后，它会更新 PR 编号并进行检查。（更多内容请参见步骤 5）

json

```json
{
  "status": "done",
  "pr": 341,
  "completedAt": 1740275400000,
  "checks": {
    "prCreated": true,
    "ciPassed": true,
    "claudeReviewPassed": true,
    "geminiReviewPassed": true
  },
  "note": "All checks passed. Ready to merge."
}
```

Step 3: Monitoring in a loop  
步骤 3：循环监控

A cron job runs every 10 minutes to babysit all agents. This pretty much functions as an improved Ralph Loop, more on it later.  
每 10 分钟运行一次定时任务，监控所有代理。这基本上相当于一个改进版的 Ralph Loop，稍后会详细介绍。

But it doesn't poll the agents directly — that would be expensive. Instead, it runs a script that reads the JSON registry and checks:  
但它不会直接轮询代理——那样成本太高。相反，它会运行一个脚本来读取 JSON 注册表并进行检查：

bash  
狂欢

```bash
.clawdbot/check-agents.sh
```

The script is 100% deterministic and extremely token-efficient:  
该脚本具有 100% 确定性，并且令牌效率极高：

- Checks if tmux sessions are alive - Checks for open PRs on tracked branches - Checks CI status via gh cli - Auto-respawns failed agents (max 3 attempts) if CI fails or critical review feedback - Only alerts if something needs human attention  
- 检查 tmux 会话是否处于活动状态 - 检查跟踪分支上是否存在未解决的 PR - 通过 gh cli 检查 CI 状态 - 如果 CI 失败或收到关键审查反馈，则自动重启失败的代理（最多尝试 3 次） - 仅当需要人工干预时才发出警报

I'm not watching terminals. The system tells me when to look.  
我不会盯着终端看。系统会告诉我什么时候该看。

Step 4: Agent Creates PR  
第四步：代理人创建公关稿

The agent commits, pushes, and opens a PR via `gh pr create --fill`. At this point I do NOT get notified — a PR alone isn't done.  
代理提交、推送代码，并通过 `gh pr create --fill` 命令创建一个 PR。此时我不会收到通知——仅仅创建一个 PR 是不够的。

Definition of done (very important your agent knows this):  
完成的定义（您的经纪人务必了解这一点）：

- PR created - Branch synced to main (no merge conflicts) - CI passing (lint, types, unit tests, E2E) - Codex review passed - Claude Code review passed - Gemini review passed - Screenshots included (if UI changes)  
- 公关稿已创建 - 分支已同步到主分支（无合并冲突） - CI 通过（lint、types、单元测试、端到端测试） - 法典审查通过 - Claude 代码审查通过 - Gemini 审核通过 - 附上屏幕截图（如果用户界面发生变化）

Step 5: Automated Code Review  
步骤 5：自动化代码审查

Every PR gets reviewed by three AI models. They catch different things:  
每份公关稿都会由三个人工智能模型进行审核。它们会发现不同的问题：

- Codex Reviewer — Exceptional at edge cases. Does the most thorough review. Catches logic errors, missing error handling, race conditions. False positive rate is very low.  
    Codex 审核员 ——擅长处理各种极端情况，审核极其全面，能够发现逻辑错误、缺失的错误处理和竞态条件，误报率极低。
    
- Gemini Code Assist Reviewer — Free and incredibly useful. Catches security issues, scalability problems other agents miss. And suggests specific fixes. No brainer to install.  
    Gemini Code Assist Reviewer—— 免费且功能强大。它能发现其他工具忽略的安全问题和可扩展性问题，并提供具体的修复建议。安装它绝对物超所值。
    
- Claude Code Reviewer — Mostly useless - tends to be overly cautious. Lots of "consider adding..." suggestions that are usually overengineering. I skip everything unless it's marked critical. It rarely finds critical issues on its own but validates what the other reviewers flag.  
    Claude 代码审查器 ——基本没用——过于谨慎。它会提出很多“考虑添加……”之类的建议，而这些建议通常都是过度设计。除非被标记为“严重”，否则我都会跳过它的所有代码。它很少能独立发现严重问题，但会验证其他审查员标记的问题。
    

All three post comments directly on the PR.  
三人均直接在公关稿上发表评论。

Step 6: Automated Testing  
步骤 6：自动化测试

Our CI pipeline runs a heavy amount of automated tests:  
我们的持续集成流水线运行大量的自动化测试：

- Lint and TypeScript checks - Unit tests - E2E tests - Playwright tests against a preview environment (identical to prod)  
- Lint 和 TypeScript 检查 - 单元测试 - 端到端测试 - Playwright 在预览环境（与生产环境相同）中进行测试

I added a new rule last week: if the PR changes any UI, it must include a screenshot in the PR description. Otherwise CI fails. This dramatically shortens review time — I can see exactly what changed without clicking through the preview.  
我上周添加了一条新规则：如果 PR 修改了任何 UI，则必须在 PR 描述中包含屏幕截图。否则持续集成 (CI) 会失败。这大大缩短了审核时间——我无需点击预览即可准确了解更改内容。

Step 7: Human Review  
步骤 7：人工审核

Now I get the Telegram notification: "PR #341 ready for review."  
现在我收到 Telegram 通知：“PR #341 已准备好审核。”

By this point:  
截至目前：

- CI passed - Three AI reviewers approved the code - Screenshots show the UI changes - All edge cases are documented in review comments  
CI 通过 三位人工智能评审员批准了该代码。 - 屏幕截图显示了用户界面的变化 所有极端情况均已在评审意见中记录。

My review takes 5-10 minutes. Many PRs I merge without reading the code — the screenshot shows me everything I need.  
我的代码审查只需要 5-10 分钟。很多 PR 我都不看代码就合并了——截图已经包含了所有我需要的信息。

Step 8: Merge  
步骤 8：合并

PR merges. A daily cron job cleans up orphaned worktrees and task registry json.  
PR 合并。每日定时任务清理孤立的工作树和任务注册表 JSON 文件。

## 

The Ralph Loop V2  
拉尔夫环路 V2

This is essentially the Ralph Loop, but better.  
这基本上就是拉尔夫环线，但更好。

The Ralph Loop pulls context from memory, generate output, evaluate results, save learnings. But most implementations run the same prompt each cycle. The distilled learnings improve future retrievals, but the prompt itself stays static.  
Ralph 循环从内存中提取上下文，生成输出，评估结果，并保存学习成果。但大多数实现方式在每个循环中都运行相同的提示。提炼出的学习成果可以改进未来的检索，但提示本身保持不变。

Our system is different. When an agent fails, Zoe doesn't just respawn it with the same prompt. She looks at the failure with full business context and figures out how to unblock it:  
我们的系统有所不同。当代理出现故障时，Zoe 不会简单地用相同的提示重新启动它。她会结合完整的业务上下文分析故障，并找出解决方法：

- Agent ran out of context? "Focus only on these three files."  
    特工信息混乱？“只关注这三个文件。”
    
- Agent went the wrong direction? "Stop. The customer wanted X, not Y. Here's what they said in the meeting."  
    代理人走错方向了？“停。客户想要的是 X，不是 Y。这是他们在会议上说的。”
    
- Agent need clarification? "Here's customer's email and what their company does."  
    客服需要进一步说明吗？“这是客户的邮箱地址以及他们公司的业务内容。”
    

Zoe babysits agents through to completion. She has context the agents don't — customer history, meeting notes, what we tried before, why it failed. She uses that context to write better prompts on each retry.  
Zoe 全程指导客服人员完成任务。她掌握着客服人员所不了解的信息——客户历史记录、会议记录、之前的尝试以及失败的原因。她利用这些信息，在每次重试时编写出更完善的提示语。

But she also doesn't wait for me to assign tasks. She finds work proactively:  
但她也不会等我分配任务。她会主动寻找工作：

- Morning: Scans Sentry → finds 4 new errors → spawns 4 agents to investigate and fix  
    早晨： 扫描哨兵系统 → 发现 4 个新错误 → 生成 4 名特工进行调查和修复
    
- After meetings: Scans meeting notes → flags 3 feature requests customers mentioned → spawns 3 Codex agents  
    会后： 扫描会议记录 → 标记客户提到的 3 个功能请求 → 生成 3 个 Codex 代理
    
- Evening: Scans git log → spawns Claude Code to update changelog and customer docs  
    晚上： 扫描 git 日志→启动 Claude Code 更新变更日志和客户文档
    

I take a walk after a customer call. Come back to Telegram: "7 PRs ready for review. 3 features, 4 bug fixes."  
我打完客户电话后出去散步。回到 Telegram 上：“7 个 PR 已准备好审核。3 个新功能，4 个 bug 修复。”

When agents succeed, the pattern gets logged. "This prompt structure works for billing features." "Codex needs the type definitions upfront." "Always include the test file paths."  
当代理成功时，该模式会被记录。“这种提示结构适用于计费功能。”“Codex 需要预先提供类型定义。”“务必包含测试文件路径。”

The reward signals are: CI passing, all three code reviews passing, human merge. Any failure triggers the loop. Over time, Zoe writes better prompts because she remembers what shipped.  
奖励信号包括：持续集成 (CI) 通过、所有三个代码审查均通过、人工合并。任何失败都会触发循环。随着时间的推移，Zoe 能写出更好的提示信息，因为她记住了哪些内容已经发布。

## 

Choosing the Right Agent  
选择合适的经纪人

Not all coding agents are equal. Quick reference:  
并非所有编码代理都一样。快速参考：

Codex is my workhorse. Backend logic, complex bugs, multi-file refactors, anything that requires reasoning across the codebase. It's slower but thorough. I use it for 90% of tasks.  
Codex 是我的主力工具。后端逻辑、复杂的 bug、多文件重构，以及任何需要跨代码库推理的任务，它都能胜任。虽然速度慢，但非常彻底。我 90% 的任务都用它完成。

Claude Code is faster and better at frontend work. It also has fewer permission issues, so it's great for git operations. (I used to use this more to drive day to day, but Codex 5.3 is simply better and faster now)  
Claude Code 在前端开发方面速度更快、性能更佳。它的权限问题也更少，因此非常适合 Git 操作。（我以前更多地用它来处理日常工作，但现在 Codex 5.3 显然更好更快。）

Gemini has a different superpower — design sensibility. For beautiful UIs, I'll have Gemini generate an HTML/CSS spec first, then hand that to Claude Code to implement in our component system. Gemini designs, Claude builds.  
Gemini 拥有另一种超能力——设计感。为了打造美观的用户界面，我会先让 Gemini 生成 HTML/CSS 规范，然后交给 Claude Code，让他将其集成到我们的组件系统中。Gemini 负责设计，Claude 负责构建。

Zoe picks the right agent for each task and routes outputs between them. A billing system bug goes to Codex. A button style fix goes to Claude Code. A new dashboard design starts with Gemini.  
Zoe 会为每个任务挑选合适的代理人，并在他们之间分配任务成果。计费系统的一个漏洞提交给了 Codex。按钮样式的修复工作交给了 Claude Code。新的仪表盘设计由 Gemini 负责。

## 

How to Set This Up  
如何设置

Copy this entire article into OpenClaw and tell it: "Implement this agent swarm setup for my codebase."  
将本文全部复制到 OpenClaw 中，并告诉它：“为我的代码库实现此代理群设置。”

It'll read the architecture, create the scripts, set up the directory structure, and configure cron monitoring. Done in 10 minutes.  
它会读取架构，创建脚本，设置目录结构，并配置定时任务监控。10分钟即可完成。

No course to sell you.  
我们不向你推销任何课程。

## 

The Bottleneck Nobody Expects  
意想不到的瓶颈

Here's the ceiling I'm hitting right now: RAM.  
目前我遇到的瓶颈是：内存。

Each agent needs its own worktree. Each worktree needs its own `node_modules`. Each agent runs builds, type checks, tests. Five agents running simultaneously means five parallel TypeScript compilers, five test runners, five sets of dependencies loaded into memory.  
每个代理都需要自己的工作树。每个工作树都需要自己的 `node_modules` 目录。每个代理运行构建、类型检查和测试。五个代理同时运行意味着五个并行的 TypeScript 编译器、五个测试运行器以及五组加载到内存中的依赖项。

My Mac Mini with 16GB tops out at 4-5 agents before it starts swapping — and I need to be lucky they don't try to build at the same time.  
我的 Mac Mini 配备 16GB 内存，最多只能运行 4-5 个代理，之后就会开始切换——而且我得祈祷它们不会同时尝试构建。

So I bought a Mac Studio M4 max with 128GB RAM ($3,500) to power this system. It arrives end of March and I'll share if it's worth it.  
所以我买了一台配备 128GB 内存的 Mac Studio M4 Max（3500 美元）来运行这套系统。它三月底到货，到时候我会分享一下是否物有所值。

## 

Up Next: The One-Person Million-Dollar Company  
接下来：一人百万美元公司

We're going to see a ton of one-person million-dollar companies starting in 2026. The leverage is massive for those who understand how to build recursively self-improving agents.  
从 2026 年开始，我们将看到大量由一人创立的百万美元公司涌现。对于那些懂得如何构建递归式自我改进代理的人来说，杠杆效应是巨大的。

This is what it looks like: an AI orchestrator as an extension of yourself (like what Zoe is to me), delegating work to specialized agents that handle different business functions. Engineering. Customer support. Ops. Marketing. Each agent focused on what it's good at. You maintain laser focus and full control.  
它看起来是这样的：一个人工智能编排器作为你的延伸（就像 Zoe 之于我一样），将工作分配给负责不同业务职能的专精代理。工程、客户支持、运营、市场营销，每个代理都专注于自己擅长的领域。而你则能保持高度专注和完全掌控。

The next generation of entrepreneurs won't hire a team of 10 to do what one person with the right system can do. They'll build like this — staying small, moving fast, shipping daily.  
下一代创业者不会雇佣十个人去做一个人用合适的系统就能完成的事情。他们会这样构建——保持小规模，快速行动，每日交付。

There's so much AI-generated slop right now. So much hype around agents and "mission controls" without building anything actually useful. Fancy demos with no real-world benefits.  
现在市面上充斥着太多人工智能生成的垃圾产品。围绕智能体和“任务控制”的炒作铺天盖地，却没有真正开发出任何有用的东西。花里胡哨的演示，却没有任何实际意义。

I'm trying to do the opposite: less hype, more documentation of building an actual business. Real customers, real revenue, real commits that ship to production, and real loss too.  
我正努力反其道而行之：减少炒作，更多地记录实际的业务构建过程。真实的客户、真实的收入、真正上线运行的代码，以及真实的亏损。

What am I building? Agentic PR — a one-person company taking on the enterprise PR incumbents. Agents that help startups get press coverage without a $10k/month retainer.  
我正在打造什么？Agentic PR——一家挑战传统企业公关公司的单人公司。我们帮助初创公司获得媒体报道，而无需每月支付 1 万美元的顾问费。

If you want to see how far I take this, follow along.  
想知道我会把这件事做到什么程度，那就跟着一起看吧。