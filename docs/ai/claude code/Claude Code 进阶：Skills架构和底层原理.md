最近Skills有多火，我就不说了。

2024年，[Prompt工程](https://zhida.zhihu.com/search?content_id=269300476&content_type=Article&match_order=1&q=Prompt工程&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzAzMDYzMTQsInEiOiJQcm9tcHTlt6XnqIsiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjkzMDA0NzYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.aO0DisPxN0iIAmH_HRoj7dJ0zn52kS9MfWDGNtLZCD4&zhida_source=entity)。

2025年，[上下文工程](https://zhida.zhihu.com/search?content_id=269300476&content_type=Article&match_order=1&q=上下文工程&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzAzMDYzMTQsInEiOiLkuIrkuIvmloflt6XnqIsiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjkzMDA0NzYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.1L_zAa8jWhty0xH-FQK6ekMsywX0Flv2Ayou4KbpywY&zhida_source=entity)。

2026年，Skills工程。



这是一个 Skills 开放标准，由 **Anthropic 发布并推动作为开放标准**，旨在让不同 AI 平台都能实现一个通用的 “**Agent Skills**” 格式。

Anthropic 真是 AI 标准的制定者，前有 **MCP** 协议，现在又弄出了 **Agent Skills** 标准。

Agent Skills 现在已经被主流的 AI 开发工具全面支持了，我看 **OpenAI、Google、Cursor** 等 AI 厂商都已经跟进并支持 Skills 了。

比如，我刚在 Claude 写完 Skills，直接就可以复制到 CodeX 中使用，100% 兼容。



Skills 的架构

Skills 在代码执行环境中运行，它具有**文件系统访问、bash 命令和代码执行**功能。

这是 Skills 的架构图：

![img](https://pic1.zhimg.com/v2-a5926433d308b17a90c675dbd0e70f20_1440w.jpg)

可以这样理解，Skills 相当于是虚拟机上的目录，Claude 可以使用计算机上导航文件相同的 bash 命令与它们交互。

## Skills 的工作原理

Skills 是通过**渐进式披露**来高效管理上下文，这张图演示了 Claude 如何加载和使用 PDF 处理 skill 的方式：



![img](https://picx.zhimg.com/v2-6dbf8e5b3dfd8966477776a2fdac0953_1440w.jpg)



这种动态加载方式，确保只有相关的 Skill 内容占据上下文窗口。

### 第 1 步：发现 Skills（始终加载）

Claude 在启动时，代理只会加载每个可用技能的 `SKILL.md` 中的元数据，比如：**名称和描述**，用来判断它什么时候可能用得上。

元数据格式如下：

```text
---
name: pdf-processing
description: 从 PDF 文件中提取文本和表格、填充表单、合并文档。在处理 PDF 文件或用户提及 PDF、表单或文档提取时使用。
---
```

这种轻量级的加载方式，意味着我们可以集成大量的 Skills 而不会产生上下文成本，Claude 只知道每个 Skill 的存在以及何时使用它。



### 第 2 步：激活 Skills（触发时加载）

当任务匹配到某个技能的**描述**时，代理才会把完整的 `SKILL.md` 指令加载进上下文里。

~~~text
 PDF 处理

## 快速入门

使用 pdfplumber 从 PDF 中提取文本：

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

有关高级表单填充，请参阅 [FORMS.md](FORMS.md)。
~~~

### 第 3 步：执行 Skills（按需加载）

代理会按照 `SKILL.md` 中的指令来操作，必要时还会加载 `references` 目录中引用的文件，或者运行 `scripts` 目录下打包好的脚本及代码。

Skills 通过**渐进式披露**这种方式，可以让代理按需调取更多上下文，从而执行得飞快。

### 渐进式披露成本

渐进式披露确保任何给定时间，只有相关内容占据上下文窗口，这是它的成本：

| 步骤          | 加载时间   | 令牌成本                 |
| ------------- | ---------- | ------------------------ |
| 第 1 步：发现 | 始终加载   | 每个 Skill 约 100 个令牌 |
| 第 2 步：激活 | 触发时加载 | 不到 5k 个令牌           |
| 第 3 步：执行 | 按需加载   | 实际上无限制             |

## SKILL.md 的文件结构

每一个 Skill 都必须要有一个 `SKILL.md` 文件，它是一个 `Markdown` 格式的文件，包含 YAML 前置元数据和 Markdown 指令。

参考格式如下：

```text
---
name: your-skill-name
description: 简要描述此 Skill 的功能以及何时使用它
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---

# Skill 名称

## 指令
[Claude 要遵循的清晰、分步指导]

## 示例
[使用此 Skill 的具体示例]
```

在 `SKILL.md` 的顶部，必须加上前置元数据，主要是 `name` 和 `description` 这 2 个元数据，其他的都是可选的。

| 字段          | 是否必填 | 约束条件                                                     |
| ------------- | -------- | ------------------------------------------------------------ |
| name          | 是       | 最多 64 个字符；只能包含小写字母、数字和连字符；不能以连字符开头或结尾。 |
| description   | 是       | 最多 1024 个字符；不能为空；用于描述该技能的功能以及适用场景。 |
| license       | 否       | 许可证名称，或指向随技能一起提供的许可证文件的引用。         |
| compatibility | 否       | 最多 500 个字符；用于说明环境要求，例如目标产品、系统依赖、网络访问等。 |
| metadata      | 否       | 用于附加元数据的任意键值映射。                               |
| allowed-tools | 否       | 技能可使用的预批准工具列表，以空格分隔（实验性功能）。       |

另外，Markdown 中的实际指令，**对结构和内容没有特别限制**。

Agent Skills 这一套机制，表面看只是多了一个 `SKILL.md` 文件，实际上背后是一整套 **Agent 能力组织方式的升级**。

Agent Skills 把**提示词、工具、脚本、资源**全部收敛到一个标准化目录里，再通过「**渐进式披露**」的方式按需加载，这一点对**上下文成本和执行效率**的提升非常明显。
