

## Chrome DevTools MCP 实战

```
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest

```

用法

```
用Chrome浏览器打开 http://www.yinfans.me/,然后通过 chrome devtools mcp 完成以下任务:
1. 截取页面截图
2. 提取所有链接
3. 分析页面结构
4. 获取页面性能数据
```



安装官方插件市场

```
claude /plugin marketplace add anthropics/skills
```

![image-20260123232434976](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260123232434976.png)

推荐安装的插件如下

![image-20260123232613291](https://typora-1305062402.cos.ap-beijing.myqcloud.com/image-20260123232613291.png)

也可以通过命令添加

```
#从市场安装:
claude /plugin install frontend-design


# 直接从GitHub仓库安装
claude plugin install github:user/repo


# 安装本地插件
claude plugin install ./my-plugin

# 或使用完整路径
claude plugin install /path/to/my-plugin
```







## code-simplifier 

code-simplifier 的核心使命只有一条：**在绝对保证原有功能不变的前提下，让代码变得更清晰、更统一、更易读。**

```
claude plugin install code-simplifier

```

**使用方式：**

在完成一轮较长的 coding session 后，直接对 Claude 说：

> Use the code-simplifier agent to clean this up

它就会自动读取你项目的风格指南，然后开始大扫除。