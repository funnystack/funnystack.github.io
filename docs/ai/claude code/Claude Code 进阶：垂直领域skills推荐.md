

## 编程开发的skills精选

- ‌**`frontend-design`**‌: 一个官方提供的通用型 Skill，能根据你的描述或需求，自动生成或重新设计前端界面，非常适合不擅长设计的开发者。‌‌

- ‌**`UI UX Pro Max`**‌: 针对前端开发者，提供比官方 `frontend-design` 更丰富的设计风格选择，对审美要求高的用户是刚需。‌‌






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



## 自媒体的skills精选

- 

- ‌**`seo-review`**‌: 用于对网站进行 SEO 审查，帮助优化网站在搜索引擎中的表现。‌‌

  

- ‌**`content-creator`**‌: 能根据你提供的关键词，自动生成博客文章或内容草稿，是内容创作者和 SEO 人员的得力助手。‌‌



- ‌**`skill-prompt-generator`**‌: 一个硬核工具，主要用于生成高质量的图片生成提示词（Prompt），其输出质量直接决定了 AI 绘图的效果。‌‌



- ‌**X 平台发布 Skill**‌: 实现了从 Claude Code 到 X（原 Twitter）平台的自动化内容发布，思路可延伸至其他社交媒体平台。‌‌

  

### ‌**Humanizer-zh**‌

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



### Ceeon/videocut-skills

把视频剪辑塞进终端，动动嘴就成

这个项目直接给 Claude Code 装了一把“智能剪刀”。它把视频剪辑这活儿彻底给“口语化”了。你只需要在终端里跟 Claude 唠句嗑：“帮我把 video.mp4 最后的 5 秒掐了，再把片头切掉 10 秒”，剩下的脏活累活它就自个儿去后台跑了。这种感觉，就像是配了个随叫随到的实习生，你只管出主意，他负责干活，关键是人家还没怨言。

**带你跑一遍**： 装好 Skill 之后，你就可以像这样“调戏” Claude 了：

在 Claude Code 界面里直接下指令 

```
# I want tocutvideo.mp4 from 00:01:00 to 00:02:00 and save it as clip.mp4.

# 帮我把 test.mov 里的高潮部分剪出来（如果你之前描述过哪部分是高潮的话）
```



### videocut-skills

这个开源项目是一个**视频剪辑 Skill**，叫 videocut-skills。

它能够辅助你完成视频处理工作，比如识别视频中的口误、静音片段以及语气词啥的。

通过简单的指令让 AI 自动处理这些多余的内容，提高剪辑效率。

这个 Skill 集成了多种自动化功能，比如使用 **Whisper 模型生成字幕**，并支持通过词典进行纠错。

它**利用 FFmpeg 进行底层的视频剪辑操作**，确保了处理速度和质量。智能体还具备自我更新的能力，可以根据用户的使用习惯不断优化剪辑规则。

对于需要频繁制作口播类视频的创作者来说，这个工具提供了一套完整的工作流。

从环境安装到最终成片，只需在对话框中输入相应命令即可完成复杂的剪辑任务。

还挺有意思的。

**如何使用**

**① 下载 Skills**

```
# 克隆到 Claude Code skills 目录 git clone https://github.com/Ceeon/videocut-skills.git ~/.claude/skills/videocut
```

**② 安装环境**

打开 Claude Code，输入 **/videocut:安装** ，AI 会自动安装依赖、下载模型，大概 5GB。

安装完成你就能用下面这些能力处理你的原素材了。

```
开源地址：https://github.com/Ceeon/videocut-skills
```



### 小红书发布 Skills

这个开源的 Skill 是我从 linux.do 上发现的。

这个 Skill 可以**帮你撰写、制作并发布小红书笔记。**它结合了内容生成与视觉渲染技术，能够根据你设定的主题自动生成符合平台风格的文案。

```
开源地址：https://github.com/comeonzhj/Auto-Redbook-Skills
```







https://zhuanlan.zhihu.com/p/1996896276856455574
