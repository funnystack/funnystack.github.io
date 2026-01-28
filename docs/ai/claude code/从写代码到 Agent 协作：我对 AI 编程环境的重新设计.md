# 从写代码到 Agent 协作：我对 AI 编程环境的重新设计

过去一年，我明显感觉到一件事：

> **AI 编程工具，已经从“效率加成”，变成了“工程基础设施”。**

但真正决定你效率上限的，并不是“你有没有用 AI”，而是——**你用的是不是一套长期可控、可演进的组合**。

所以我花了一段时间，把主流 AI 编程方案几乎都试了一遍。

最终，我定下了这套配置：

> **Trae（IDE） + Claude Code（Agent 工具） + GLM-4.7（核心模型）**

这不是“参数党”的选择，而是一个**工程视角下的理性取舍**。

------

### 一、为什么我放弃了 Cursor

Cursor 很流行，也确实好用。 但我最终放弃它，原因很简单：

> **核心能力被锁在“高级会员 + 官方模型”体系里。**

对架构师来说，这意味着两件事：

- 模型不可控
- 长期成本不可控

而我更需要的是：

- 能自由切换模型
- 能接入国产 / 开源模型
- 能作为 **Agent 核心** 长期演进

Cursor 并不适合这个目标。

------

### 二、我最终选择这套组合的判断逻辑

我选工具时，只看三点：

1. **是否贴合真实开发流程**
2. **是否支持 Agent 化协作**
3. **模型是否可替换、可升级**

Trae 负责 IDE 层体验 Claude Code 负责 Agent 能力 GLM-4.7 负责模型性能与成本控制

三者职责清晰，没有重叠。

------

### 三、Claude Code：它为什么不只是“写代码”

很多人看到 `Claude Code`，会以为它只是个代码工具。

但从 2025 年预览版到现在，它已经发生了质变：

- 支持完整代码上下文理解
- 支持多轮任务拆解
- 支持 MCP / SDK 通信
- 本质上已经是一个 **通用编程 Agent**

**它解决的不是“写得快”，而是“协作成本”。**

这对复杂系统、架构设计、跨模块重构尤其重要。

------

### 四、为什么我选择 GLM-4.7 作为核心模型

一句话总结：**在可控成本下，它是目前最适合工程使用的编码模型之一。**

- Code Arena 开源模型第一梯队
- 原生兼容 Claude Code、Cline 等 20+ 工具
- 可直接接入 VS Code / IDEA
- 对中文语境、工程注释、架构理解非常友好

我更看重的是： **它不像“实验模型”，而是“工程模型”。**

------

### 五、完整配置步骤（核心版）

#### 1️⃣ 获取 GLM-4.7 API Key

- 登录智谱 AI 开放平台（[open.bigmodel.cn/](https://link.juejin.cn/?target=https%3A%2F%2Fopen.bigmodel.cn%2F)）
- 控制台 → API 密钥管理 → 创建密钥
- **注意保管，不要提交到仓库**

#### 2️⃣ 安装 Claude Code

```bash
bash

 体验AI代码助手
 代码解读
复制代码# 1. 通过npm全局安装Claude Code（需提前安装Node.js 18.0.0及以上版本）
npm install -g @anthropic-ai/claude-code

# 2. 验证安装是否成功（显示版本号即代表安装完成）
claude --version

# 3. 诊断工具运行环境（自动检测依赖是否缺失，若有报错可根据提示修复）
claude doctor
```

#### 3️⃣ 一键配置（强烈推荐）

```bash
bash

 体验AI代码助手
 代码解读
复制代码npx @z_ai/coding-helper
```

选择：

- 中文
- 国内套餐
- Claude Code
- 粘贴 API Key

#### 4️⃣ 可选：手动确认配置

若需自定义参数或验证配置是否生效，可手动检查并修改 Claude Code 的默认配置文件`.claude\settings.json`，确保 GLM-4.7 为默认模型：

```json
json

 体验AI代码助手
 代码解读
复制代码{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "此处替换为你的GLM-4.7 api_key",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic", // 智谱官方接口地址，勿修改
    "API_TIMEOUT_MS": "3000000", // 超时时间设置为50分钟，适配长任务
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1, // 关闭非必要流量，提升响应速度
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-4.5-air", // 轻量模型，用于简单任务
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-4.7", // 默认模型，用于常规编码任务
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-4.7" // 高阶模型，用于复杂推理任务
  }
}
```

------

### 六、真实使用体验 & 适合谁

这套配置**不适合所有人**。它更适合：

- 架构师
- 中高级工程师
- 需要做系统设计 / 重构 / 多模块协作的人

如果你只是写简单 CRUD，确实有点“杀鸡用牛刀”。

------

### 写在最后

2026 年，AI 编程的分水岭已经很清晰了：

> **不是“你用不用 AI”， 而是你有没有一套属于自己的 AI 工程体系。**

工具会变，模型会换，但**判断标准一旦建立，就能长期复用。**

🚀 立即拼团薅羊毛：智谱 GLM Coding 订阅链接直达，20 + 编程工具无缝支持，越拼越划算→[www.bigmodel.cn/glm-coding?…](https://link.juejin.cn/?target=https%3A%2F%2Fwww.bigmodel.cn%2Fglm-coding%3Fic%3DNRP5X8Y1CB)

建议尽早订阅，用更低成本解锁高阶编码能力！