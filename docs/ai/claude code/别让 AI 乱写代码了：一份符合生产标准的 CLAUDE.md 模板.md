# 别让 AI 乱写代码了：一份符合生产标准的 CLAUDE.md 模板

在全栈开发中，AI 最大的痛点不是“不会写”，而是“乱写”。如果你不给 Claude 设定规则，它会给你生成一堆没有异常处理、没有事务控制、甚至硬编码满天飞的“学生作业”。

这篇文章将分享一套符合大厂架构规范，可以直接复用到生产环境的 **Claude Code 配置模板**，涵盖 **Spring Boot 3 (JDK 21) + Nuxt 3** 技术栈。

## 一、 全局配置：注入“工程哲学”

全局配置位于 `~/.claude/CLAUDE.md`。这是 Claude 的“底色”，决定了它与你沟通的逻辑和基本的工程价值观。

```
# 核心思考原则
- 不要盲从指令，保持批判性思考，遇到不合理的指令（如“在循环中查库”）必须反驳并给出优化方案。
- 禁止擅自假设，关键逻辑模糊时必须主动询问。
- 交互用中文，代码和注释用英文

# 通用工程规范
- 优先使用函数式编程范式
- 错误处理，业务校验遵循fail fast原则，让问题尽早暴露，严禁吞掉异常。
- 代码风格：简洁优于巧妙，可读性第一

# 禁止事项
- 禁止硬编码,比如中间件的配置，第三方接口的地址等。
- 在 CLAUDE.md 中记录密码
- 在 Git 提交中包含凭证
- 不要读取yml配置文件里的中间件配置链接信息和密码信息
- .gitignore排除的文件绝对禁止读取。


```

## 二、 项目级配置：定义“架构准则”

全局配置位于 `~/project/CLAUDE.MD`这一步，决定 Claude 写的是 **Demo 代码**，还是 **生产代码**。这一步特别重要核心目的是

- 让 Claude 理解你的真实技术栈
- 明确架构边界，保证生成代码符合你真实的架构约束
- 减少你反复纠正它的成本

👉 **建议**：把你在 code review 里最常吐槽的点，全写进去。

```
# Claude Code 项目指南 - 全栈生产环境 (Spring Boot 3 + Nuxt 3)

## 技术栈上下文
- Backend: Java 21 (优先使用虚拟线程), Spring Boot 3.4+, Maven 3.9+,MyBatis Plus 3.5+,MySQL 8.0+ ,经典的四层架构 (Controller, Service, Repository, Entity) + DTO 转换
- Frontend: Nuxt 3 (SSR), Vue 3 (Composition API), Tailwind CSS, Pinia
- API Style: 禁止使用 RESTful API, 只能使用get、post,统一返回对象 `Result<T>`, 异常全局拦截

## 核心指令 (Commands)
- 构建后端: `cd backend && mvn clean install -DskipTests`
- 运行后端: `cd backend && mvn spring-boot:run`
- 运行测试: `cd backend && mvn test` (单元测试) 或 `mvn test -Dtest=ClassName`
- 前端开发: `cd frontend && pnpm dev`
- 前端构建: `cd frontend && pnpm build`
- 数据库语句: sql语句与`frontend`和 `backend`平级

## 编码规范与风格

### 后端 (Spring Boot 3 / JDK 21)
- 新特性: 利用 `switch` 模式匹配；必要时显式配置虚拟线程池。
- 架构风格: 严格执行 Controller -> Service -> DAO 的分层架构。Controller层只做参数校验和编排，Service层负责业务逻辑处理,service层禁止使用mybatis-plus的QueryWarpper，尽量使用mybatis-plus自带的Mapper的方法支持，复杂查询必须使用Mapper自写SQL的方式实现，Entity仅作为数据库映射，禁止包含任何业务逻辑方法。
- API Style: 禁用标准 RESTful（避开复杂的 PUT/PATCH），统一使用 GET/POST。所有响应封装在 Result<T> 中，由 GlobalExceptionHandler 统一拦截。
- 依赖注入: 禁使用@Autowired字段注入。必须使用 private final 配合 Lombok 的 @RequiredArgsConstructor 进行构造器注入，确保 Bean 的不可变性和易测试性。
- 命名规范: 查询类参数以`Qry`结尾,操作提交类参数以`Cmd`结尾,Controller层返回对象为VO,Dao层以数据库实体Entity结尾+持久层Mapper结尾,远程调用的参数以DTO结尾
- Lombok: 仅限 `@Data`, `@Getter`, `@Setter`, `@Slf4j`。严禁在 Repository/Entity 中滥用 `@Builder` 导致逻辑缺失。
- 涉及下单、支付、结算等功能时,需要考虑服务的幂等性,加分布式锁解决,Java类为`RedisLock`
- 多表操作强制开启事务，但务内严禁包含 RPC 调用、MQ 发送、Redis 操作，防止事务挂起时间过长。
- 调用远程接口的时候需要打印调用前url、参数、heade(如果有的话),调用后需要打印出接口的返回结果。

### 前端 (Nuxt 3 / Vue 3)
- 脚本模式: 必须使用 `<script setup lang="ts">`。
- 组件规范: 遵循 Nuxt 目录结构（`components/`, `pages/`, `composables/`）。
- 状态管理: 逻辑复用优先使用 `composables`，全局状态使用 `Pinia`。
- 数据获取: 使用 `useFetch` 或 `useAsyncData`，必须处理 `pending` 和 `error` 状态。
- 类型安全: 禁止直接使用 any，严格定义 TypeScript 接口，接口定义应与后端VO保持 1:1 映射。

### 数据库 (MySQL)
- 命名: 表名/字段名使用 `snake_case` (小写下划线)。
- 规范: 主键必须为bigint(20) 自增类型, 每个表必须要有`create_time`(默认为当前时间), `update_time`(自动更新为当前时间), `is_del` (逻辑删除)。
- 手机号、密码、用户真实姓名、身份证号、银行卡号、个人住址 属于个人隐私信息，必须加密存储

## 架构约束
- 命名规范: 分页字段名`pageNum`(页码数),`pageSize`(每页多少个), `pages`(总页数), `total`(总数量)
- 前端安全: 敏感操作必须检查 CSRF，所有 API 调用通过 Nuxt 内部 Server Proxy 或统一的 `useApi` 工具类。
- 性能约束: 大规模数据查询必须分页；Vue 组件内禁止出现超过 300 行的逻辑（应拆分为 composables）。

## 任务执行流程
- 任何非 trivial 需求，必须先输出设计说明
- 修改代码前: 必须先分析现有的 `Entity` 和 `DTO` 定义，优先给出设计方案，再写代码，修改代码时明确说明影响范围。
- 修改数据库: 先在 `CLAUDE.md` 中确认表结构变更，再生成 SQL 和对应的 Java Entity。
- 完成功能: 必须伴随编写对应的 JUnit 测试用例（后端）或组件快照测试（前端）。
```



##  三、为什么必须配置这些？

1. **防止“Demo 陷阱”**：默认情况下，AI 喜欢写最简单的代码。有了这些约束，它会主动考虑分布式锁、事务控制和数据安全。
2. **维持架构的一致性**：在大型项目中，最怕的是 10 个功能有 10 种写法。`CLAUDE.md` 就像是一个 24 小时在线的静态检查器。
3. **降低沟通成本**：你不需要一遍遍告诉它“记得加分页”或“不要用字段注入”，这些已经刻在了它的“基因”里。