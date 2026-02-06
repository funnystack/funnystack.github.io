---
sidebar_position: 4
---
# Claude Code 进阶：Skills 安装与管理最佳实践

> 上一篇文章介绍了 Skills 与 MCP 的区别，本文讲点更实际的：去哪里找好用的 Skills，以及如何安装和管理本地 Skills才不会乱。

## 前言：为什么需要 Skills？

AI 辅助开发有个让人头疼的问题：同样的事情要在进行下一个项目需要反复说，比如代码规范、文档格式需要再说一遍，这样重复且低效。

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
- https://skillsmp.com/
- 最大的一个，9 万+，爬虫抓的 GitHub
- 搜索方便，什么都有，质量参差不齐
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

### 优先级建议

官方（稳）→ travisvn（实用新鲜）→ skills.sh（特定领域）→ Composio（精选收藏）

实际用起来，「官方 + travisvn」基本够打，再视情况补充 skills.sh。

## 二、Skills安装指南

### 1. 使用find-skills查找skill

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

<img src="https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203222019644.png" alt="image-20260203222019644" width="50%" />

#### 怎么用

最简单的使用方法就是在命令行操作

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

以 搜 React 性能优化 举例，我们可以搜索关键词`react performance`

```bash
npx skills find react performance
```

搜到了就安装：

```bash
npx skills add vercel-labs/agent-skills@vercel-react-best-practices
```

![image-20260203224708135](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203224708135.png)

装完之后，在前端项目命令行下运行 `claude`，然后输入：

```
根据 `vercel-react-best-practices` 技能，检查我项目里的代码并给出重构建议。
```

![image-20260203224854765](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203224854765.png)

### 2. 使用skill-creator创建skill

Anthropic 官方的"元技能"，用来创建新的 Skills。说清楚需求，它自动生成符合标准的目录结构、指令文件和脚本。企业可以基于这个构建自己的技能库，提升 Claude 在实际工作中的效率。

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

进入claude命令行后，输入指令创建技能

```
用 skill-creator 创建一个「weekly-report-generator」技能，自动生成周报。要求：1. 读取周报模板（assets/template.md）；2. 调用 Read 工具提取项目数据；3. 输出 Markdown 格式；4. 允许工具：Read、Glob；语言简洁专业。
```

![image-20260203230344175](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203230344175.png)

![image-20260203230418587](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260203230418587.png)

创建完：
1. 启用新技能（设置 → 功能 → 技能列表，勾选「weekly-report-generator」）
2. 上传测试数据，跑一下看看效果

需要改的话，重新触发：

```
用 skill-creator 更新 weekly-report-generator 技能，加一个「自动生成 PDF 附件」功能，允许工具：Write、CodeExecutor
```

### 3.编写自己的skills

#### Skill 目录结构

```
my-skill/
├── skill.json          # Skill 元数据
├── skill.md            # Skill 文档
├── api/                # API 定义(可选)
└── tools/              # 自定义工具(可选)
```

#### skill.json 示例:

```json
{
  "name": "my-custom-skill",
  "description": "我的自定义技能",
  "version": "0.0.1",
  "author": "Your Name",
  "categories": ["automation"],
  "license": "MIT",
  "skill": {
    "file": "skill.md",
    "description": "这个技能用于..."
  }
}

```

#### skill.md 示例:

```
# My Custom Skill

这个技能帮助用户快速完成[特定任务]。

## 使用场景

- 场景1:描述...
- 场景2:描述...

## 使用方式

用户只需要告诉你要完成什么,这个技能就会自动:

1. 分析需求
2. 执行步骤
3. 返回结果

## 注意事项

- 注意事项1
- 注意事项2

```

#### 安装本地 Skill:

```
# 将技能复制到 Claude Code 配置目录
cp -r my-skill ~/.claude/skills/

# 或使用安装命令
npx skills-installer install ./my-skill --client claude-code
```

## 三、Skills 组合：自动化流水线

很多人已经在用 AI 了，但往往只停留在 Skills 层。这一步确实有用，但有上限。

AI 时代拉开差距的，从来不是会不会用工具，而是谁先把"流程"交出去。

Skills 解决的是"能力稳定性"，不是"流程自动化"。真正的自动化流水线，是 Skills、Subagents、Hooks 组在一起用的。

从工程角度理解三者分工：

  - Skills：会干什么
  - Subagents：谁来干
  - Hooks：什么时候干，干完之后接着干什么

 组合起来后，AI 就不是"被动响应的工具"了，而是可以自主运行的工作流系统，优势在于：

- 状态可持续：上一步的输出会系统性地传给下一步，不是一次性消耗的对话上下文。
- 角色稳定存在：Subagents 是长期、固定职责的角色，不是每次临时"假装自己是某某专家"。
- 流程事件驱动：Hooks 决定流程如何推进和分支。人只在设计阶段介入一次，不用全程盯着。

举个技术方案评审自动化的例子。这在技术团队里很常见，也确实适合自动化。

#### 第一步：方案输入

技术方案提交后，系统级 Hook 触发，启动「方案拆解 Subagent」。

这一阶段不做判断，只做结构化拆分：背景、目标、架构方案、关键决策点、风险假设。

#### 第二步：并行评审

 方案拆解完成后，Hook 并行触发多个 Subagents，每个只承担单一职责：

  - 架构 Subagent → 调用「架构合理性 Skill」，评估系统拆分、依赖关系与演进空间
  - 稳定性 Subagent → 调用「高可用检查 Skill」，关注异常路径、降级方案、扩展能力
  - 成本 Subagent → 调用「资源与复杂度评估 Skill」，评估长期维护成本与技术负债风险

 三条评审链路同时执行、彼此隔离，避免单一视角顾此失彼。

#### 第三步：结果汇总

所有 Subagents 执行完成后，汇总 Hook 触发，启动「评审汇总 Subagent」。它只做一件事：冲突点合并、风险分级排序、决策建议生成。不重新分析事实，只对多个评审结论进行结构化收敛。

####  第四步：结论输出

最后由 Hook 根据规则完成决策分支：风险等级高于阈值就生成整改建议清单，否则生成评审通过结论。一次完整流程走完，没人需要手动决定"下一步该做什么"。



## 四、本地 Skills 怎么管

### 核心原则

本地 Skills 管理，结论是：**不是越多越好，也不是越少越好，关键是精准**。

AI 助手不会每次对话都带上所有 Skills，而是按需调用。

### 两种策略

| 管理策略 | 越多越好 | 精准管理 |
|:---|:---|:---|
| 思路 | 广泛安装，以备不时之需 | 按需安装，保持精简 |
| 优点 | 覆盖广 | **响应更快更准**，干扰少 |
| 缺点 | 1. 可能误触发不相关技能 2. 影响响应速度 3. 管理混乱 | 需要主动安装/卸载，有管理成本 |
| 推荐 | 不推荐 | **推荐** |

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





参考文章

1. [Anthropic Skills 官方仓库](https://github.com/anthropics/skills)
2. [Skill-Creator 源码目录](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
3. [Anthropic 技能创建指南](https://support.claude.com/en/articles/12512198-creating-custom-skills)
4. [Claude Skills API 文档](https://docs.claude.com/en/api/skills-guide)



