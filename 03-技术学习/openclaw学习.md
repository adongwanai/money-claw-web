装完 OpenClaw 之后，很多人站在配置文件的面前发呆。

工具散落在不同的文档里。Skill 默认全部自动加载，有些已经生效了，你却浑然不知。全部开启怕有风险，全部关闭又觉得白装一场。想要从文档和源码中拼凑出全貌，需要投入大量的时间和精力。

这篇文章来自一位开发者的实战笔记。他花了大量时间研究 OpenClaw 的 25 个工具和 53 个官方 Skill，逐一分析了每个功能的作用、潜在风险以及配置建议。文章末尾还附上了他的完整配置文件，你可以直接复制使用。

（ClawHub 上有 3000 多个第三方 Skill，不在本文讨论范围内。）

![Image](https://pbs.twimg.com/media/HCImkcha0AAsE_P?format=png&name=small)

先搞懂：工具和 Skill 的区别

很多人把这两个概念搞混，其实它们的区别非常清晰

● 工具是器官，决定了 OpenClaw “能不能”做某件事

○ read 和 write 让它能操作文件，exec 让它能执行系统命令，web_search 让它能搜索，web_fetch 让它能读取网页，browser 让它能操作浏览器。没有开启对应的工具，OpenClaw 就像缺少手脚，什么都干不了。

● Skill 是教科书，教 OpenClaw “如何组合工具”来完成任务

○ gog 教它使用 Google Workspace 处理邮件和日历，obsidian 教它管理笔记，github 教它操作代码仓库，slack 教它发送频道消息。53 个官方 Skill 覆盖了笔记、邮件、社交媒体、开发、智能家居等多个领域。

安装 Skill 会给 OpenClaw 新增权限吗？不会。

举个例子：你安装了 obsidian Skill，OpenClaw 知道了如何整理笔记。但如果没有开启 write 工具，它连文件都写不了。Skill 只是使用说明书，真正的开关在工具里。

要让 OpenClaw 通过 Skill 实际执行操作，必须满足三个条件。以“读取 Gmail”为例：

1. 配置：你是否允许 OpenClaw 执行命令？（没有 exec，它连程序都启动不了）

2. 安装：机器上是否安装了 gog 桥接工具？（没有它，OpenClaw 知道该做什么，但连不上 Google）

3. 授权：你是否登录 Google 账号并授予了权限？（没有授权，Google 不会放行）

三者缺一不可。

Skill 是说明书，实际能不能用取决于这三个条件。

同心圆架构：从核心到外围

把 25 个工具和 53 个 Skill 平铺罗列会让人 overwhelm。这位开发者用同心圆的方式来组织：

OpenClaw 同心圆架构：

第 1 层核心工具（read、write、exec）

第 2 层高级工具（browser、memory、automation）

第 3 层知识层（53 个官方 Skill）

![Image](https://pbs.twimg.com/media/HCImnAJbUAAdfUW?format=png&name=small)

第 1 层：核心能力（8 个工具）

这 8 个工具是 OpenClaw 的基础。只开启这些，OpenClaw 处于“被动响应”模式：你提问，它回答。它能读取文件、执行命令、搜索网页，但不会跨会话记住你的偏好，也不会主动推送通知。把 OpenClaw 从“聊天机器人”变成“助手”的关键在第 2 层。但没有第 1 层，第 2 层无法运转。

文件操作：read、write、edit、apply_patch  
文件作：read、write、edit、apply_patch

● read 是只读。write 和 edit 可以修改文件，apply_patch 用于应用代码变更。这四个工具是基础中的基础，大多数人会全部开启。

执行与进程管理：exec、process

● exec 让 OpenClaw 可以执行任意 shell 命令：安装包、运行脚本、管理系统。“任意”是关键词：它可以帮你装依赖，也可以执行 rm -rf 清空你的整台机器。没有 exec，大部分任务无法完成。开启了 exec 却没有防护，等于把 root 权限交了出去。

这就是为什么强烈推荐为 exec 开启审批模式：每条命令先展示给你，确认后才执行：

{ "approvals": { "exec": { "enabled": true } } }  
{ “批准”： { “exec”： { “enabled”： true }

麻烦吗？说实话，有点。但这是最基础的防护：万一 AI 判断失误，或者遭遇 Prompt Injection 攻击，这道门是最后的防线。

process 管理后台进程：查看任务列表、检查输出、终止卡死的进程。通常和 exec 一起开启。

网络访问：web_search、web_fetch

● web_search 进行关键词搜索，web_fetch 读取网页内容。两者配合，让 OpenClaw 具备了浏览互联网获取信息的能力。

第 2 层：高级能力（17 个工具）

第 1 层解决“能不能用”，第 2 层解决“用得好不好”。这些工具把 OpenClaw 从命令执行者变成真正的助手：它会记住你的偏好，控制浏览器，发送定时通知。但每增加一个工具，攻击面就扩大一分，需要权衡是否值得。

浏览器：browser、canvas、image

● browser 让 OpenClaw 可以操控 Chrome：点击按钮、填写表单、截取网页。这位开发者用它做价格对比、规格调研、加购物车。但结账这一步永远亲自操作。涉及支付的“最后一公里”从不交给 AI，这是他的底线。

● canvas 是可视化工作区，用于绘制图表和流程图。image 让 OpenClaw 具备“理解”图像的能力。

记忆：memory_search、memory_get

● 让 OpenClaw 跨会话记住信息。使用一周后，它知道这位开发者用 Astro 建博客，部署在 Azure，习惯用繁体中文。不需要每次都重新解释。用得越久，它越懂你。

多会话：sessions 系列（5 个工具）

● 同时运行多个会话处理不同任务：一个讨论产品想法，另一个调研旅行计划，互不干扰。

○ sessions_list 和 sessions_history 查看会话

○ session_status 检查状态；

○ sessions_send 和 sessions_spawn 实现会话间通信和子任务派生。

消息推送：message

● 让 OpenClaw 能向 Discord、Slack、Telegram、WhatsApp、iMessage 发送消息。

这位开发者开启了此功能，但只用于给自己发消息，从不代表他与他人沟通。原因很简单：AI 以你的名义发出的消息无法撤回。万一它误解了语境、用错了语气，或者被 Prompt Injection 诱导发送了不当内容，后果由你承担。

在他的 AI 目标管理系统中，OpenClaw 充当通信层。开启 message 后，它可以主动推送通知：每日简报、任务提醒、待办事项预警，全部发给自己。

硬件控制：nodes

● 跨设备硬件控制：远程截图、GPS 定位、摄像头访问。

这位开发者第一次看到此工具时问自己：什么时候需要 AI 自动打开我的摄像头？他想不出场景。至于截图，他可以通过 Telegram 手动发送，多一步操作但安心得多。最终选择关闭。

自动化：cron、gateway

● cron 设置定时任务

● gateway 让 OpenClaw 能够重启自身。

每天早上 6:47，他的 Telegram 会收到 OpenClaw 准备的每日简报：今天要做什么、有哪些待回复的消息、天气预报。这就是 cron 加 message 的组合效果，也是他 AI 目标管理系统的核心。

代理通信：agents_list

● 列出可用的 Agent ID。OpenClaw 支持多代理架构，但官方文档未详细说明。如果只运行单个 OpenClaw 实例，不需要这个工具。

扩展工具：llm_task、lobster  
扩展工具：llm_task、龙虾

● lobster 是工作流引擎，用于定义多步骤流程。llm_task 在工作流中插入 LLM 处理步骤。

如果你不使用工作流引擎，两个都可以跳过。

第 3 层：知识层（53 个官方 Skill）

53 个听起来很多，但扫一遍你会发现可能只有十几个与你相关。其他如外卖、智能家居、语音通话，不是说它们不好，只是不符合你的使用场景。

重要提示：捆绑的 Skill 默认自动加载。如果系统上安装了对应的 CLI 工具，Skill 就会自动激活。这不是“不装就没有”，而是“不禁用就全开”。要控制哪些 Skill 处于活动状态，使用 skills.allowBundled 白名单模式（配置示例见文末“我的配置”部分）。

ClawHub 上有 3000 多个第三方 Skill，但它们的安全风险是另一个话题。

以下按使用场景分类整理。

一、笔记类

● 4 个笔记 Skill:obsidian、notion、apple-notes、bear-notes。能否使用取决于你的部署环境。

apple-notes 和 bear-notes 只能在本地 Mac 上使用。如果 OpenClaw 跑在虚拟机里，这两个无效。

obsidian 操作本地文件。这位开发者用 Obsidian，但他的仓库在 Mac 上，而 OpenClaw 在 Azure VM 上，所以他选择在本地用 Claude Code 处理笔记，而非通过 OpenClaw。

如果希望 OpenClaw 直接管理笔记，且运行在虚拟机中，notion 是云端的，没有部署限制，是阻力最小的方案。

二、生产力类

● 两个邮件 Skill：gog 和 himalaya。

○ gog 整合整个 Google Workspace（Gmail、日历、任务、云端硬盘、文档、表格）。

○ himalaya 只用 IMAP/SMTP 收发邮件。如果你用 Google 全家桶，选 gog 更完整，而且随时可以在 Google 账号里撤销授权。

● 任务管理有 things-mac（Things 3）、apple-reminders、trello。如果已经装了 gog,Google Tasks 已经包含在内，无需额外安装。

● wacli（WhatsApp）、imsg（iMessage）、bird（X/Twitter）、slack、discord

○ 这些 Skill 让 OpenClaw 深度接入各平台，包括搜索消息历史、同步对话、管理频道。与 message 工具（只能发消息）不同，安装这些等于把你在对应平台的完整数据访问权交给了它。

这位开发者一个都没装。对外沟通的最后一步永远手动完成。

三、开发工具

● 他装了 github、tmux、session-logs。他在本地用 Claude Code 写代码，但 OpenClaw 随时可以通过 Telegram 联系。如果出门在外时 CI/CD 挂了，他直接在手机上问“查一下这个 PR 构建失败的原因”，OpenClaw 就会去拉 GitHub Actions 的错误日志并告诉他问题所在。

● 他还没装 coding-agent，但潜力巨大：可以在 OpenClaw 的虚拟机上装 Claude Code，让它调度编码任务在后台执行。

想象一下，通过 Telegram 告诉 OpenClaw：“我在 GitHub 上发现一个有趣的仓库，克隆下来研究一下，然后搭个演示站点。”它启动 Claude Code，自主执行，完成后推送通知。AI 编排 AI。他还没深入探索，但已在待办清单上。

四、密码管理

● 1password 让 OpenClaw 可以访问你的 1Password 保险库：查询密码、自动登录、填写表单。使用场景如：“帮我登录 AWS 控制台”或者“这个网站的密码是什么”。

但权限模式是全有或全无：一旦授权，整个保险库对它开放。你无法限制只能访问特定条目，你存了什么，它就能读什么。这位开发者选择不装。如果确实需要，建议创建一个“AI 专用保险库”，只放你愿意与 AI 分享的密码。

五、其他类别

以上是他主动使用或认真考虑过的类别。

剩下的如音乐播放、智能家居、图像生成、语音转文字、外卖订餐，他都没装。完整清单见文末附录。

我的 OpenClaw 配置：工具和 Skill 的选择逻辑

这位开发者的 OpenClaw 跑在 Azure VM 上，通过 Telegram 操作。配合桌面端的 Claude Code，形成移动+桌面双轨工作流：移动端随时讨论、调研、记录想法（对话历史自动同步），桌面端执行。他还用它处理日常邮件、日历、调研，以及每天早上接收 Daily Brief。

以下是他当前的配置及选择逻辑：

一、工具（25 个中开启 21 个）

他的原则很简单：想不出使用场景的，一律关闭。

{ "tools": { "allow": [ "read", "write", "edit", "apply_patch", "exec", "process", "web_search", "web_fetch", "browser", "image", "memory_search", "memory_get", "sessions_list", "sessions_history", "sessions_send", "sessions_spawn", "session_status", "message", "cron", "gateway", "agents_list" ], "deny": ["nodes", "canvas", "llm_task", "lobster"] }, "approvals": { "exec": { "enabled": true } } }  
{ “tools”： { “allow”： [ “read”， “write”， “edit”， “apply_patch”， “exec”、“process”、“web_search”、“web_fetch”、“browser”、“image”、“memory_search”、“memory_get”、“sessions_list”、“sessions_history”、“sessions_send”、“sessions_spawn”、“session_status”、“message”、“cron”、“gateway”， “agents_list” ]， “deny”： [“nodes”， “canvas”， “llm_task”， “lobster”] }， “approvals”： { “exec”： { “enabled”： true } } }

开启 21 个，关闭 4 个：

● nodes（想不出场景）

● canvas（不需要）

● llm_task/lobster（不用工作流引擎）

● exec 开启审批模式

● message 只给自己发消息。

二、Skill（53 个中开启 9 个）

如前所述，捆绑的 Skill 默认自动加载。他用 allowBundled 白名单限制只加载需要的：

{ "skills": { "allowBundled": [ "gog", "github", "tmux", "session-logs", "weather", "summarize", "clawhub", "healthcheck", "skill-creator" ] } }  
{ “技能”： { “allowBundled”： [ “gog”、“github”、“tmux”、“session-logs”、“weather”、“summarize”、“clawhub”、“healthcheck”、“skill-creator” ] } }

简单说：gog 处理邮件和日历，github 管理仓库，其余是 Daily Brief 和系统管理的工具 Skill。

如何用 AI Agent 实现任务自动化

到这里，OpenClaw 开始从聊天机器人变成基础设施。

● cron（定时调度）加 message（推送通知）的组合，把它变成了在你睡觉时也能工作的自动化引擎。

● 模式永远相同：触发条件 + 执行动作 + 结果投递。定义何时运行、做什么、结果发到哪里。以下是他实际使用的自动化场景：

○ 每日简报：每天早上 6:47，他的 Telegram 会收到简报：今天的日程安排、需要回复的待处理邮件、天气预报、以及夜间是否有 CI/CD 故障。这一个自动化替代了他喝咖啡前查看五个应用的习惯。

○ 邮件分类：每天两次，OpenClaw 扫描收件箱，按紧急程度分类，发送摘要。通讯邮件直接归档。需要处理的事项附上一行摘要标记出来。他把每天 30 分钟的收件箱管理压缩到了 5 分钟。

○ CI/CD 监控：当 GitHub Actions 工作流失败时，OpenClaw 读取错误日志，判断可能原因，推送 Telegram 消息带诊断结果。他曾在排队买咖啡时用手机修复了生产环境问题。

○ 内容调研：每天，OpenClaw 收集特定子版块的热门讨论、Hacker News 线程、他关注的 RSS 源，整理成潜在写作选题摘要。它不替他写作，只帮他发现值得写的内容。

设置并不复杂，每个自动化就是一个 cron 条目触发一个 prompt，prompt 告诉 OpenClaw 用哪些工具、结果发到哪里。

困难的不是配置，而是想清楚哪些日常流程值得自动化。从每天摩擦最大的那一个开始，把它跑通，再逐步添加。

开始配置你的 OpenClaw

你不需要全部 25 个工具。53 个捆绑 Skill 默认全开，用 allowBundled 只保留需

![Image](https://pbs.twimg.com/media/HCImqYXaoAAinoO?format=png&name=small)

要的。

打开你的 openclaw.json，遵循三个原则：

1. 想不出使用场景的，先关掉

2. 能力越大，管控越严：exec 开启审批，message 只发给自己

3. 最后一公里永远手动：结账、发消息、公开发言，任何不可撤销的操作留给自己

上面的配置可以作为起点，复制它，然后根据你的需求裁剪。

常见问题

![Image](https://pbs.twimg.com/media/HCImxeDa8AAUTWP?format=png&name=small)

1、Skill 会改变 OpenClaw 的权限吗？

不会。Skill 只是说明书，实际能力由 tools.allow 控制。

2、1password Skill 能读取我所有密码吗？

是的。一旦授权，整个保险库对它开放，你存了什么，它就能读什么。

3、如何撤销 OpenClaw 的 Google 访问权限？

Google 账号 → 安全性 → 具有账号访问权限的第三方应用 → 找到 gog → 移除访问权限。

4、ClawHub 上的第三方 Skill 安全吗？

不要默认认为安全，安装前务必查看 GitHub 仓库，可以用openclaw doctor检查

5、为什么有 25 个工具？

官方文档列出 18 个。这位开发者通过阅读源码发现了 25 个。多出的包括会话相关工具、agents_list、以及文档未提及的工作流引擎工具（llm_task、lobster）

6、OpenClaw 和 ChatGPT 的区别是什么？

ChatGPT 是聊天工具，OpenClaw 是 Agent，区别在于对话结束后会发生什么：

● 即使“同步”的含义也不同：LLM 应用的同步意味着你在手机和桌面能看到同一段对话历史。

● OpenClaw 的同步意味着对话变成你电脑文件夹里的文件，其他工具可以直接读取并继续处理。一个是“可见”，一个是“可操作”。

如果你只想聊天，ChatGPT 够用；如果你希望 AI 在对话结束后继续工作，你需要 OpenClaw 这样的 Agent。

7、如何基于 OpenClaw 实现 AI 任务自动化？

组合 cron（定时调度）和 message（推送通知）。

● OpenClaw 按计划执行任务，把结果推送到你的消息平台。每天早上 6:47，这位开发者会收到每日简报：今日任务、待回复事项、天气预报。

● 除了定时推送，常见的自动化场景包括：邮件分类与优先级摘要、CI/CD 故障监控、定时收集写作素材、行业新闻摘要。

本质上，任何能拆解为“触发条件 + 执行步骤”的任务都可以自动化。

8、使用 OpenClaw 需要会编程吗？

日常使用不需要编程，用自然语言对话即可，比如：“查一下今天的邮件”、“设个明天早上 9 点的提醒”，直接说就行。

但 OpenClaw 是开源项目，安装和配置有学习曲线，可以部署到云虚拟机或本地安装。

出于安全考虑，建议使用专用机器而非日常主力机。如果用 Claude Code 这类 AI CLI 工具，它可以协助完成设置过程，节省大量时间。

附录：完整参考

全部 25 个工具

![Image](https://pbs.twimg.com/media/HCInI-7a0AA4hhy?format=png&name=medium)

全部 53 个 Skill

![Image](https://pbs.twimg.com/media/HCInW2CacAA2SXB?format=png&name=large)

工具组（快捷方式）：

![Image](https://pbs.twimg.com/media/HCInexyakAA5qGm?format=png&name=small)

关于关于杰森 AI 出海的 AI 出海圈

我们专注于 AI 出海的实战经验分享，提供：

● 课程：推特 IP 涨粉变现实战

● 咨询：3 次 1V1 深度咨询

● 圈子：3 年社群陪伴  
● 圈子：三年社区陪伴

● 资讯：AI 技术趋势

想了解更多？加入我们的出海圈，一起在 AI 时代掘金全球市场。

本文针对 yuwenhao的教程，进行了深度调研整理

https://yu-wenhao.com/en/blog/openclaw-tools-skills-tutorial/

如果觉得有价值，欢迎转发给更多需要的朋友。