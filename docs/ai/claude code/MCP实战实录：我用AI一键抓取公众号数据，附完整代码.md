# MCP实战实录：我用AI一键抓取公众号数据，附完整代码

> 假如AI会"用鼠标"，也会"复制粘贴"，世界会变成什么样？

上篇文章我们介绍了MCP的基础知识和安装方法。今天，我们直接上干货**用真实的案例展示MCP到底能做什么**。

从网页仿写到数据抓取，从数据库查询到API调用，让我们看看当AI拥有了"操作现实世界"的能力，会发生什么神奇的事情。

## 一、开场：一个震撼的实战效果

先给大家看一个实际应用场景：

**任务**：抓取某公众号前3页所有文章的详细数据（标题、阅读量、点赞量、发布时间），并保存到Excel表格。

**传统做法**：

1. 手动打开每一篇文章
2. 复制粘贴数据到Excel
3. 花费时间：30-60分钟

**用MCP的做法**：
1. 告诉AI需求
2. AI自动调用Chrome DevTools MCP完成抓取
3. 花费时间：**30秒**

![image-20260207122755501](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207122755501.png)

![image-20260207123732678](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207123732678.png)

这就是MCP的魅力——**让AI真正"动手"帮你干活**。

## 二、Chrome DevTools MCP：网页自动化神器

Chrome DevTools Protocol（CDP）的MCP服务，是**底层、仅面向Chrome系的调试工具**，无额外封装，主打原生调试/监控能力。

### 安装配置

```bash
# 安装
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest

# 卸载
claude mcp remove chrome-devtools

# 验证安装
claude mcp list
```

### 实战案例1：页面仿写

**需求**：仿写极客时间的首页风格。

**Prompt**：
```
用Chrome浏览器打开 https://time.geekbang.com/
然后进行仿写，保持页面风格和样式一致，输出静态html即可。
```

**执行效果**：

![image-20260207121920315](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207121920315.png)

AI会自动：
1. 打开目标网页
2. 截图分析页面结构
3. 提取样式和布局信息
4. 生成对应的HTML代码

**应用场景**：快速原型设计、竞品分析、UI设计参考

### 实战案例2：页面性能分析

**需求**：分析豆瓣首页的性能指标。

**Prompt**：
```
用Chrome浏览器打开 https://www.douban.com/
然后通过 chrome devtools mcp 完成以下任务:
1. 截取页面截图
2. 提取所有链接
3. 分析页面结构
4. 获取页面性能数据
```

**执行效果**：

![image-20260207122329629](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207122329629.png)

AI会自动调用Chrome DevTools的各种能力，一次性完成多个分析任务。

**应用场景**：SEO优化、性能监控、竞品分析

### 实战案例3：自动化数据抓取（重点）

**需求**：抓取公众号多页文章数据。

**Prompt**：
```
用Chrome浏览器打开这个链接: [公众号链接]

然后通过 chrome devtools mcp 完成:
1. 获取第1、2、3页每篇文章的详细数据
2. 包括标题、阅读量、点赞量、发布时间等
3. 保存到 Excel 表格中
```

**执行过程**：

![image-20260207122755501](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207122755501.png)

![image-20260207123732678](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207123732678.png)

AI会自动：
1. 打开目标页面
2. 分析页面结构，定位文章列表
3. 提取每篇文章的数据
4. 处理分页逻辑
5. 生成Excel文件

**应用场景**：舆情监控、竞品分析、数据收集

## 三、MySQL MCP：让AI看懂你的数据库

安装命令：

```bash
# Using npm
npm install -g @benborla29/mcp-server-mysql

# Using pnpm
pnpm add -g @benborla29/mcp-server-mysql

# 启动该MCP服务
npx @benborla29/mcp-server-mysql
```

为Claude Code添加该MCP（项目级配置推荐）：

```bash
# 下列连接参数配置成需要连接的数据库
claude mcp add your_mcp_name \
  -e MYSQL_HOST="127.0.0.1" \
  -e MYSQL_PORT="3306" \
  -e MYSQL_USER="root" \
  -e MYSQL_PASS="your_password" \
  -e MYSQL_DB="your_database" \
  -e ALLOW_INSERT_OPERATION="false" \
  -e ALLOW_UPDATE_OPERATION="false" \
  -e ALLOW_DELETE_OPERATION="false" \
  --scope project \
  -- npx @benborla29/mcp-server-mysql
```

> **为什么推荐项目级配置？**
>
> 不同项目的数据库不一样，针对不同项目配置不同的MCP参数，没必要全局安装。

执行成功后，会在项目根目录下生成 `*.mcp.json` 配置文件：

![image-20260207125514386](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207125514386.png)

### 实战案例：智能数据查询

![image-20260207133939574](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260207133939574.png)

你可以直接用自然语言查询数据库：

- "查询本月注册用户的平均年龄"
- "找出消费金额前10的用户"
- "分析最近一周的订单趋势"

AI会自动：
1. 理解你的查询意图
2. 查看表结构
3. 生成SQL语句
4. 执行查询并返回结果

**应用场景**：数据分析、报表生成、业务洞察

## 四、Apidog MCP：OpenAPI文档终于不是摆设了

虽然大多数MCP服务都主打通用型功能，但**Apidog MCP Server**另辟蹊径，专注服务API开发，正在彻底改变我们与接口文档打交道的方式。

### 专为API打造的AI接口助手

与"泛用型"的上下文插件不同，Apidog MCP Server做到了**接口专注、语义智能**：

🔹 **直接对接Apidog项目、公共文档，或本地OpenAPI文件**

🔹 **自然语言查询API内容**
> "接口 `/users` 的响应结构是什么？"

🔹 **离线也能工作**，本地缓存OpenAPI规范

🔹 **保持上下文感知**，确保AI给出的建议符合当前项目规范

### 2分钟配置

在Cursor编辑器中配置Apidog MCP Server：

1. 打开Cursor，点击设置齿轮图标
2. 左侧选择 **"MCP"**
3. 点击 **"+ Add new global MCP server"**
4. 粘贴以下配置（替换 `<access-token>` 和 `<project-id>`）：

```json
{
  "name": "Apidog MCP",
  "url": "https://mcp.apidog.com",
  "accessToken": "<access-token>",
  "projectId": "<project-id>"
}
```

### 实际效果

- **自动生成TypeScript接口定义**：AI直接生成，准确无误
- **构建Python客户端SDK**：AI秒懂接口结构，马上帮你写好

不需要再反复翻阅文档，AI就像你项目里最懂接口的"全栈拍档"。

## 五、对比：有MCP vs 无MCP

让我们用一个表格总结MCP带来的改变：

| 场景 | 传统方式 | 使用MCP后 |
|------|----------|-----------|
| **网页数据抓取** | 手动复制或写爬虫脚本 | 告诉AI需求，自动完成 |
| **数据库查询** | 手写SQL，需要了解表结构 | 自然语言描述，AI生成并执行 |
| **API接口开发** | 来回翻阅Swagger文档 | AI直接查询并生成代码 |
| **文件操作** | 手动在文件管理器中操作 | AI直接读写搜索文件 |
| **网络信息获取** | 手动搜索多个网站 | AI实时调用搜索能力 |

**核心差异**：传统方式：**你服务工具 **  MCP方式：**工具服务你**

## 六、总结与展望

通过以上实战案例，我们可以看到MCP的核心价值：

### ✅ 已经能做到的
- 网页自动化操作
- 数据库智能查询
- API接口语义化查询
- 文件系统操作
- 实时网络搜索

### 🚀 未来可能的方向
- 更多垂直领域的MCP服务
- MCP服务之间的组合调用
- 企业级MCP私有化部署
- MCP与AI Agent的深度结合

## 七、行动建议

如果你还没体验过MCP，建议按以下顺序尝试：

1. **第一步**：安装Chrome DevTools MCP，体验网页自动化
2. **第二步**：根据你的工作流，选择数据库或文件系统MCP
3. **第三步**：探索更多特色服务，解锁AI的无限潜力

> **MCP不仅仅是一个技术协议，它正在构建一个开放、标准化的AI工具生态。**



**参考资料：**

- [Playwright MCP vs Chrome DevTools MCP vs Chrome MCP 深度对比](https://www.cnblogs.com/clnchanpin/p/19190994)
- [Claude Code 接入 MySQL 实战：让 AI 看懂你的表结构和数据](https://blog.csdn.net/weixin_46436257/article/details/155200001)
- [Apidog MCP：让AI掌握你的接口全貌](https://juejin.cn/post/7514947261395550227)
