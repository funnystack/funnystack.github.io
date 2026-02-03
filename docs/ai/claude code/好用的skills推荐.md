##

上一篇文章详细的介绍了skills与mcp的区别



### 深入解析 find-skills

它本质上是一个给AI助手（如Claude Code、Cursor等）使用的“插件”或“指南”。安装后，你可以用自然语言让AI帮你搜索技能，它会自动执行搜索命令（如 `npx skills find data analysis`），然后为你展示结果并协助安装。

这个工具由Vercel推出，旨在解决不同AI助手之间技能不互通的问题，建立了一个开放的技能共享标准。

### 💡 如何找到你需要的技能

最实用的建议是**直接使用 `find-skills` 进行探索**。例如，向你的AI助手提出以下请求，它就能帮你发现相关技能：

- “**帮我搜索一个前端设计的skill**”
- “**查找关于React组件开发的技能**”
- “**有没有能优化CSS代码的技能？**”

总的来说，`find-skills` 是你探索技能世界的“地图”，而 `frontend-design` 是地图上的一个具体“地点”。



目前市面上能找到的 Skill 主要有这些：

**1.[http://skillsmp.com](https://link.zhihu.com/?target=http%3A//skillsmp.com)**

[http://skillsmp.com](https://link.zhihu.com/?target=http%3A//skillsmp.com)：目前体量最大（已超 9 万+），靠爬虫全量聚合 GitHub 技能，搜索方便但质量参差不齐，适合找灵感或边缘需求时用。

**2.[https://github.com/anthropics/skills](https://link.zhihu.com/?target=https%3A//github.com/anthropics/skills)**

官方出品，数量少但最稳、最纯净，几乎不会踩坑，强烈推荐作为新手/生产环境的首选和基准。

**3. [https://github.com/ComposioHQ/awesome-claude-skills](https://link.zhihu.com/?target=https%3A//github.com/ComposioHQ/awesome-claude-skills)**

**人工精选+分类清晰，质量较高但更新可能没那么勤，属于值得收藏但不常逛的靠谱列表型资源。**

**4.**[https://github.com/travisvn/awesome-claude-skills](https://link.zhihu.com/?target=https%3A//github.com/travisvn/awesome-claude-skills)***\*
\****

这是由**社区成员 Travis**维护的另一个精选列表，社区驱动的 awesome 榜单里星最高的一个，挑选偏激进/实用向，数量和新鲜度都优于 Composio 版，适合想快速找到高赞黑魔法的人。

**5.[http://skills.sh](https://link.zhihu.com/?target=http%3A//skills.sh)
**

Vercel 官方合作+高质量前端/部署方向技能，安装量排行榜很实用，生态正在快速起势，React/Next.js 重度用户几乎必看。

[http://6.skillstore.io](https://link.zhihu.com/?target=http%3A//6.skillstore.io)

走安全审计+一键安装路线，技能经过筛选和版本锁定，适合对代码卫生和团队一致性要求高的场景，但整体体量和热度暂时还追不上前几个。

一句话总结优先级逻辑：

先官方（稳）→ travisvn（实用新鲜）→ skills.sh（生态+特定领域强）→ Composio（精选收藏）→ skillstore（安全团队向）→ skillsmp（海量但杂）。

实际使用中很多人是 官方 + travisvn 这两家组合打天下，再视领域补充 skills.sh 或其他。





### 官方与高人气 Skills

- ‌**`skill-creator`**‌: 这是 Anthropic 官方提供的“元技能”，专门用于帮助用户快速创建新的 Skills。只需用自然语言描述你的需求（例如“我想做一个将 PDF 转为 PPT 的技能”），它就能自动生成符合标准的目录结构、指令文件和脚本，是入门和定制化开发的绝佳起点。‌

定位核心：skill-creator 是 Anthropic Skills 生态的元技能，定义了自定义技能的标准化结构和开发流程，是将 Claude 定制为“专业智能体”的核心工具；
架构规范：所有自定义技能需遵循“SKILL.md 为核心 + 可复用资源目录”的结构，元数据（name/description）是 Claude 触发技能的关键；
实战价值：通过 init_skill.py 初始化、编写标准化资源、package_skill.py 打包的流程，可快速创建符合 Anthropic 规范的自定义技能，实现 Claude 能力的个性化扩展。
skill-creator 的设计本质是“将模糊的自然语言指令转化为结构化、可复用的技能包”，通过标准化和流程化，让 Claude 能稳定、一致地完成复杂的专属任务，而非依赖单次的自然语言提示。对于企业而言，基于 skill-creator 构建贴合自身业务的技能库，可大幅提升 Claude 在实际工作中的落地效率。



- ‌**`frontend-design`**‌: 一个官方提供的通用型 Skill，能根据你的描述或需求，自动生成或重新设计前端界面，非常适合不擅长设计的开发者。‌‌

  

- ‌**`Planning-with-files`**‌: 受益于 Manus 的 Agent 方法论，这个 Skill 特别擅长处理多步骤的复杂任务。一个有趣的用法是用它来协调和管理其他 Skills 的工作流程，相当于为你的 AI 助手配了一个项目经理。‌‌

  

### 垂直场景实用 Skills

- ‌**`UI UX Pro Max`**‌: 针对前端开发者，提供比官方 `frontend-design` 更丰富的设计风格选择，对审美要求高的用户是刚需。‌‌

  

- ‌**`seo-review`**‌: 用于对网站进行 SEO 审查，帮助优化网站在搜索引擎中的表现。‌‌

  

- ‌**`content-creator`**‌: 能根据你提供的关键词，自动生成博客文章或内容草稿，是内容创作者和 SEO 人员的得力助手。‌‌



- ‌**`skill-prompt-generator`**‌: 一个硬核工具，主要用于生成高质量的图片生成提示词（Prompt），其输出质量直接决定了 AI 绘图的效果。‌‌



- ‌**X 平台发布 Skill**‌: 实现了从 Claude Code 到 X（原 Twitter）平台的自动化内容发布，思路可延伸至其他社交媒体平台。‌‌

  

## ‌**Humanizer-zh**‌

本工具能够识别并修复 **24 种** AI 写作痕迹，分为四大类内容模式、语言和语法模式、风格模式、交流模式和填充词等。让你文章 不仅"干净"，更"鲜活"，避免 AI 模式只是基础，好的写作需要真实的人类声音。

项目地址：https://github.com/op7418/Humanizer-zh

通过 npx 一键安装（推荐）

```
npx skills add https://github.com/op7418/Humanizer-zh.git
```

重启 Claude Code 或重新加载 skills 后，在对话中输入：

```
/humanizer-zh
```

如果安装成功，该技能将被激活。

在 Claude Code 中，你可以通过以下方式使用 Humanizer：

直接调用技能

```
/humanizer-zh 请帮我人性化以下文本：

[粘贴你的 AI 生成文本]
```

 在对话中使用

```
请用 humanizer 帮我改写这段话，让它更自然：

这个项目作为我们团队致力于创新的证明。此外，它展示了我们在不断演变的技术格局中的关键作用。
```

处理文件内容

```
/humanizer-zh 请人性化 article.md 文件中的内容
```









#### 5.3.1 change-summary（PR 描述/发布说明生成器）

```
---
name: Change Summary Generator
description: Generate PR description / release notes / changelog from code changes. Use when user asks for summary, changelog, release notes, or PR description.
user-invocable: true
context: fork
allowed-tools:
  - Read
  - Grep
---

# Change Summary

目标：把变更整理成“可直接粘贴到 PR/发布说明”的结构化内容。

输出契约（必须遵守）：
1) Summary（3~5 行）
2) User Impact（谁会受影响）
3) Risk & Rollback（风险与回滚策略）
4) Test Evidence（跑了什么/没跑为什么）
5) Notes（兼容性/配置变更/迁移说明）

```

#### 5.3.2 api-doc-generator（API 文档生成器）

```
---
name: API Documentation Generator
description: Generate API documentation from routes/types/comments. Use when user asks to create API docs or document endpoints.
user-invocable: true
context: fork
allowed-tools:
  - Read
  - Grep
  - Write
---

# API Documentation

对每个 endpoint 输出（必须完整）：
- Method / Path / Description
- Auth（是否需要登录/权限）
- Request（Headers/Params/Body Schema）
- Response（Success Schema + Error Codes）
- curl Example（可复制）

额外要求：
- 如果找不到路由定义，说明你查了哪些文件/模式（便于我补上下文）

```

#### 5.3.3 db-migration-helper（数据库迁移助手：必须可回滚）

```
---
name: Database Migration Helper
description: Help create safe and reversible database migrations with explicit up/down. Use when user needs to modify database schema.
user-invocable: true
context: fork
allowed-tools:
  - Read
  - Write
  - Bash
---

# Database Migration Helper

原则：
1) 必须可回滚（必须有 Down）
2) 高风险操作必须显式标注（drop/rename/backfill）
3) 考虑性能与锁表风险（索引/大表 DDL）

输出契约：
1) Migration Plan（步骤 + 风险点）
2) Up Migration（完整内容）
3) Down Migration（完整内容）
4) Rollout Notes（上线建议：分批/灰度/监控项）
5) Rollback Steps（回滚命令/验证点）

```

#### 5.3.4 code-review-assistant（结构化代码审查：可执行清单）

```
---
name: Code Review Assistant
description: Perform comprehensive code reviews focusing on security, correctness, performance, and maintainability. Use when user asks to review code or audit changes.
user-invocable: true
context: fork
allowed-tools:
  - Read
  - Grep
---

# Code Review Assistant

审查范围：
- Correctness（边界/异常/并发）
- Security（鉴权/注入/敏感信息）
- Performance（热路径/IO/复杂度）
- Maintainability（命名/结构/重复/可测试性）

输出契约（必须遵守）：
1) Summary（一段话）
2) Must Fix（每条：文件 + 问题 + 影响 + 修复建议）
3) Should Fix（同上）
4) Nice to Have（可选）
5) Test Suggestions（建议补哪些测试）

```

5.4 与 Hooks/Subagents 组合：把流程做成“装配线”
如果只做 Skills，会得到“更稳定的模板”；但如果把 Skills、Subagents、Hooks 组合起来，就能得到“自动化流水线”，一个最容易落地的团队组合拳：

Subagent：test-writer（岗位）绑定 Test Generator 类 Skill（SOP）
Hook：PostToolUse(Write|Edit) 自动格式化 + 最小测试（不阻断）
Hook：Stop 做最终门禁（测试/lint/git status）
Skill：change-summary 在最后生成 PR 描述（结构化输出）
这样基本能做到：每次交付都带 测试证据、风险与回滚、可直接粘贴的 PR 总结。





对于管理本地的skills，核心结论是：**数量并非越多越好，但也不是越少越好，关键在于“精准”**。AI助手并不会在每次对话时“傻乎乎地”带上所有skills的元数据，而是采用更智能的方式。

为了帮助你高效管理，我制作了一个表格来对比两种管理思路：

| 管理策略     | **“越多越好”策略**                                           | **“精准管理”策略**                              |
| :----------- | :----------------------------------------------------------- | :---------------------------------------------- |
| **核心理念** | 广泛安装，以备不时之需                                       | 按需安装，保持工具包精简                        |
| **潜在优点** | 技能库全面，覆盖范围广                                       | **响应更快、更准**，减少无关信息干扰            |
| **主要缺点** | 1. **增加干扰**：可能误触发不相关技能。 2. **占用资源**：虽每个token小，但总量会影响上下文长度和响应速度。 3. **管理混乱**：难以记住所有技能功能。 | 需要根据项目周期主动安装/卸载，有一定管理成本。 |
| **推荐场景** | **不推荐**作为常规策略。                                     | **推荐**作为最佳实践，适合绝大多数用户。        |

### 🛠️ 核心机制与实用建议

理解下面的机制，你就知道如何权衡了：

1. **智能调用，而非“全量携带”**
   高质量的skills生态（如 `skills.sh`）通常采用**按需调用**的机制。AI助手会根据你的对话内容，智能判断并**只激活相关的一小部分技能**来辅助回答，而不是把所有技能的说明文本都塞进每次对话的上下文里。这意味着安装很多技能，通常不会直接拖慢每次对话。
2. **管理成本与“技能污染”**
   技能过多会增加你的**管理负担**。你需要记住安装了哪些技能，有时AI可能会错误地匹配或推荐不相关的技能，影响对话的精准度。
3. **关于Token占用**
   虽然每个技能描述不大，但安装的元数据（如技能名称、描述、参数列表）会占用本地存储空间。更重要的是，如果同时激活过多技能，其总token数会挤占宝贵的对话上下文窗口，可能影响AI处理核心问题的能力。

### 💡 如何有效管理你的技能

基于以上分析，建议你采用以下方法：

- **按项目周期管理**：在开始一个新项目或聚焦于某一领域（如数据分析、UI设计）时，集中安装一批相关的技能。项目结束后，可以卸载掉近期不再需要的。
- **用 `find-skills` 主动探索**：善用这个“技能搜索引擎”，在需要时精确查找，而不是预先安装一堆可能用不上的。
- **定期审视**：每隔一段时间，查看一下已安装的技能列表，清理掉那些很久没用或功能重复的技能。

总结来说，把本地skills看作你开发工具箱里的工具——一个为当前工作精心挑选、顺手好用的工具包，远比一个塞满生锈工具的巨大仓库要高效得多。





Anthropic Skills 官方仓库：https://github.com/anthropics/skills
Skill-Creator 源码目录：https://github.com/anthropics/skills/tree/main/skills/skill-creator
Anthropic 技能创建指南：https://support.claude.com/en/articles/12512198-creating-custom-skills
Claude Skills API 文档：https://docs.claude.com/en/api/skills-guide





