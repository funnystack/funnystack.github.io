# Claude Code 进阶：Skills 安装与管理最佳实践

> 上一篇文章介绍了 Skills 与 MCP 的区别，本文讲点更实际的：去哪里找好用的 Skills，以及本地 Skills 怎么管才不会乱。

## 前言：为什么需要 Skills？

AI 辅助开发有个让人头疼的问题：同样的事情要反复说。代码规范要解释一遍，文档格式又要解释一遍，下次又忘了。重复且低效。

Skills 就是用来解决这个问题的。执行一条命令 `npx skills add vercel-labs/agent-skills`，就能像装 npm 包一样把某个能力装到本地。下次需要的时候，AI 直接调用，不用再重复解释。

Vercel 推出的这套技能共享标准，把各种能力拆成标准化模块。开发者可以像搭积木一样组合使用——根据实际使用反馈，大概能省下 60% 的重复工作。

## 一、去哪里找 Skills

### 方法 1：用 find-skills

`find-skills` 本质上是一个"插件"，安装后你的 AI 助手就能帮你搜索技能。你用自然语言说需求，它自己执行搜索命令，然后帮你展示结果。

直接用它就行。比如对 AI 助手说：

- 「帮我找个前端设计的 skill」
- 「找 React 相关的技能」
- 「有没有优化 CSS 的？」

![image-20260203225141301](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203225141301.png)

把它当成探索技能世界的地图。

### 方法 2：各种 Skills 市场

现在能找到 Skill 的渠道主要有这些：

#### 1. skillsmp.com
- 最大的一个，9 万+，爬虫抓的 GitHub
- 搜索方便，什么都有
- 质量嘛，参差不齐
- 适合找灵感或者边缘需求

#### 2. Anthropic 官方仓库
- https://github.com/anthropics/skills
- 官方出品，数量不多但稳
- 几乎不会踩坑
- 新手和生产环境首选

#### 3. ComposioHQ 精选列表
- https://github.com/ComposioHQ/awesome-claude-skills
- 人工精选，分类清晰
- 质量可以
- 更新没那么勤，适合收藏着偶尔看看

#### 4. Travis 社区精选
- https://github.com/travisvn/awesome-claude-skills
- 社区成员 Travis 维护的
- 挑选偏激进实用向，新鲜度比 Composio 好
- 想找"黑魔法"可以看看

#### 5. skills.sh
- https://skills.sh/
- Vercel 官方合作，前端/部署方向
- 安装量排行榜很实用
- React/Next.js 重度用户必看

---

### 优先级建议

官方（稳）→ travisvn（实用新鲜）→ skills.sh（特定领域）→ Composio（精选收藏）

实际用起来，「官方 + travisvn」基本够打，再视情况补充 skills.sh。

## 二、必须装的 Skills

### 1. find-skills

Vercel Labs 出的，堪称"AI 助手的包管理器"。接到新任务找不到对应 Skill 时，用它搜一下。

#### 能干嘛

通过 `find-skills`，AI 助手能快速掌握：

- React 和 Next.js 最佳实践
- PR 代码审查
- 自动化测试
- 文档生成
- UI/UX 设计
- DevOps 部署

每个技能都是对应领域专家做的，持续更新，拿到的都是最新的技术栈和验证过的方法论。

#### 怎么装

```bash
npx skills add https://github.com/vercel-labs/skills --skill find-skills
```

![image-20260203221951941](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203221951941.png)

<img src="https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203222019644.png" alt="image-20260203222019644" style="zoom:50%;" />

#### 怎么用

**手工命令行**

```bash
# 搜索技能
npx skills find [关键词]

# 安装技能
npx skills add <owner/repo@skill>

# 卸载技能
npx skills remove <owner/repo@skill>

# 检查更新
npx skills check

# 更新所有技能
npx skills update
```

**举例：搜 React 性能优化**

```bash
npx skills find react performance
```

搜到了就安装：

```bash
npx skills add vercel-labs/agent-skills@vercel-react-best-practices
```

![image-20260203224708135](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203224708135.png)

装完之后，在前端项目目录下运行 `claude`，然后输入：

```
根据 `vercel-react-best-practices` 技能，检查我项目里的代码并给出重构建议。
```

![image-20260203224854765](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203224854765.png)

---

### 2. skill-creator

Anthropic 官方的"元技能"，用来创建新的 Skills。说清楚需求，它自动生成符合标准的目录结构、指令文件和脚本。

#### 核心定位

- **元技能**：定义了自定义技能的标准化结构
- **架构规范**：SKILL.md 核心 + 可复用资源目录
- **本质**：把自然语言需求转成结构化的技能包

企业可以基于这个构建自己的技能库，提升 Claude 在实际工作中的效率。

#### 安装方式一：Claude Code 命令行

```bash
# 添加官方插件市场
/plugin marketplace add anthropics/skills

# 安装依赖包（skill-creator 属于 example-skills 子包）
/plugin install example-skills@anthropic-agent-skills
```

验证：
- `/plugin list` 看看有没有 example-skills 和 skill-creator
- 重启 Claude Code 确保生效

#### 安装方式二：手工安装

从官方仓库获取：https://github.com/anthropics/skills/tree/main/skills/skill-creator

放进对应目录：

```bash
# 项目级（仅当前项目）
.claude/skills/skill-creator/

# 用户级（所有项目）
~/.claude/skills/skill-creator/
```

#### 实战：创建自定义技能

**创建**

```
用 skill-creator 创建一个「weekly-report-generator」技能，自动生成周报。要求：1. 读取周报模板（assets/template.md）；2. 调用 Read 工具提取项目数据；3. 输出 Markdown 格式；4. 允许工具：Read、Glob；语言简洁专业。
```

![image-20260203230344175](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203230344175.png)

![image-20260203230418587](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203230418587.png)

创建完：
1. 启用新技能（设置 → 功能 → 技能列表，勾选「weekly-report-generator」）
2. 上传测试数据，跑一下看看效果

**修改已有技能**

需要改的话，重新触发：

```
用 skill-creator 更新 weekly-report-generator 技能，加一个「自动生成 PDF 附件」功能，允许工具：Write、CodeExecutor
```

## 三、Skills 组合：自动化流水线

只用 Skills，得到的是"更稳定的模板"。把 Skills、Subagents、Hooks 组合起来，得到的是"自动化流水线"。

### 一个组合方案

| 组件 | 角色 | 功能 |
|:---|:---|:---|
| **Subagent** | test-writer（岗位） | 绑定 Test Generator 类 Skill（SOP） |
| **Hook** | PostToolUse(Write\|Edit) | 自动格式化 + 最小测试（不阻断） |
| **Hook** | Stop | 最终门禁（测试/lint/git status） |
| **Skill** | change-summary | 生成 PR 描述（结构化输出） |

这样每次交付都有：测试证据、风险与回滚方案、可直接用的 PR 总结。

## 四、本地 Skills 怎么管

### 核心原则

本地 Skills 管理，结论是：**不是越多越好，也不是越少越好，关键是精准**。

AI 助手不会每次对话都带上所有 Skills，而是按需调用。

### 两种策略

| 管理策略 | **「越多越好」** | **「精准管理」** |
|:---|:---|:---|
| **思路** | 广泛安装，以备不时之需 | 按需安装，保持精简 |
| **优点** | 覆盖广 | **响应更快更准**，干扰少 |
| **缺点** | 1. 可能误触发不相关技能<br>2. 影响响应速度<br>3. 管理混乱 | 需要主动安装/卸载，有管理成本 |
| **推荐** | 不推荐 | **推荐** |

### 底层机制

**1. 按需调用，不是全量携带**

高质量的 Skills 生态（如 `skills.sh`）采用按需调用。AI 助手根据对话内容判断，只激活相关的一小部分技能。

**2. 管理成本与"技能污染"**

技能太多会增加管理负担。你得记住装了啥，AI 有时也会错误匹配不相关的技能。

**3. Token 占用**

虽然每个技能描述不大，但元数据（名称、描述、参数）会占用本地存储。激活太多技能会挤占对话上下文窗口。

### 管理建议

- **按项目周期管理**：开始新项目或聚焦某个领域时，集中安装一批相关的。项目结束后，卸载不用的
- **用 `find-skills` 主动探索**：需要时精确查找，别预先装一堆可能用不上的
- **定期审视**：隔段时间看看已安装列表，清理很久没用或功能重复的

---

### 结语

本地 Skills 就像工具箱。一个精心挑选、顺手的工具包，远比塞满生锈工具的巨大仓库高效。



参考文章

Anthropic Skills 官方仓库：https://github.com/anthropics/skills
Skill-Creator 源码目录：https://github.com/anthropics/skills/tree/main/skills/skill-creator
Anthropic 技能创建指南：https://support.claude.com/en/articles/12512198-creating-custom-skills
Claude Skills API 文档：https://docs.claude.com/en/api/skills-guide




