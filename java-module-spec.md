## 模块（module）

以 **样板(my-template)** 模块为例：

### 1.1. POM 模块定义

```xml
<groupId>com.example.my</groupId>
<artifactId>my-template</artifactId>  
<version>1.0.0</version>
```

### 1.2. 模块包结构

模块基本包名（对应 src/main/java 下的包结构）：

```
com.example.my.template  
├── api/                # API 数据传输接口定义（如果需要暴露接口）
│   └── dto/            # 命令、查询、VO对象定义
│       ├── TemplateTaskDetailDTO.java
│       ├── TemplateTaskVO.java
│       ├── CreateTemplateTaskCmd.java
│       └── TemplateTaskQry.java
├── adapter/            # 适配层代码
│   ├── controller/     # web入口
│   │   └── TemplateTaskController.java
│   ├── event/          # 应用事件入口
│   │   └── TemplateTaskEventListener.java
│   ├── message/        # 消息入口
│   │   └── TemplateTaskMessageHandler.java
│   └── scheduler/      # 定时任务
│       └── TemplateTaskScheduler.java
├── app/                # 应用服务层：Service层接口在这个包中定义
│   ├── converter/      # 领域-DTO 转换器  
│   │   ├── TemplateTaskVOConverter.java
│   │   └── TemplateTaskConverter.java
│   ├── impl/           # 服务实现 
│   │   └── TemplateTaskServiceImpl.java
│   └── TemplateTaskService.java 
├── domain/             # 领域模型和业务逻辑：业务层域对象在这里定义
│   ├── gateway/        # 工厂、网关层接口定义  
│   │   └── TemplateTaskGateway.java
│   ├── event/          # 领域事件  
│   │   ├── TemplateTaskCreatedEvent.java
│   │   └── TemplateTaskDeletedEvent.java
│   └── TemplateTask.java
├── infrastructure/     # 基础设施层  
│   ├── config/         # 模块配置：业务域中的properties配置类和Configuration类在这里定义
│   │   ├── TemplateAutoConfiguration.java
│   │   └── TemplateProperties.java
│   ├── converter/      # DO-领域 转换器，xxxxConverter
│   │   └── TemplateTaskDOConverter.java
│   ├── gateway/        # 工厂、网关接口实现
│   │   └── TemplateTaskGatewayImpl.java
│   ├── persistence/    # 数据持久化  
│   │   ├── entity/     # 数据库实体  
│   │   │   └── TemplateTaskDO.java
│   │   ├── mapper/     # MyBatis Mappers  
│   │   │   └── TemplateTaskMapper.java
│   │   └── repository/ # 仓储实现
│   │       ├── impl/
│   │       │   └── TemplateTaskDORepositoryImpl.java
│   │       └── TemplateTaskDORepository.java
│   └── rpc/            # 外部调用  
│       └── OauthServiceApi.java
└── common/             # 公共组件（可选）
    ├── error/          # 错误定义：异常、错误码
    └── util/           # 工具类  
```

---

## 2. 命名规范

### 2.1. 类命名规范

类名使用 UpperCamelCase 风格，但以下情形例外：DO / BO / DTO / VO / AO / PO / UID 等。

* 正例：ForceCode / UserDO / HtmlDTO / XmlService / TcpUdpDeal / TaPromotion
* 反例：forcecode / UserDo / HTMLDto / XMLService / TCPUDPDeal / TAPromotion

### 2.2. Service/DAO 层方法命名规约

* 获取单个对象的方法用 `get` 做前缀。
* 获取多个对象的方法用 `list` 做前缀，复数结尾，如：`listObjects`。
* 分页获取多个对象的方法用 `query` 做前缀
* 获取统计值的方法用 `count` 做前缀。
* 插入的方法用 `save`/`insert` 做前缀。
* 删除的方法用 `remove`/`delete` 做前缀。
* 修改的方法用 `update` 做前缀。

### 2.3. 领域模型命名规约

* 数据对象：`xxxDO`，`xxx` 即为数据表名。
* 数据传输对象：`xxxDTO`，`xxx` 为业务领域相关的名称。
* 展示对象：`xxxVO`，`xxx` 一般为网页名称。
* POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 `xxxPOJO`。

### 2.4. 接口命名规约

* Service 层接口**不加** `I` 前缀，与实现类保持语义一致。
  * 正例：`TemplateTaskService` / `TemplateTaskServiceImpl`
  * 反例：`ITemplateTaskService` / `TemplateTaskServiceImpl`
* 网关接口以 `Gateway` 结尾，如：`TemplateTaskGateway`
* RPC 接口以 `Api` 结尾，如：`OauthServiceApi`

### 2.5. 常量与枚举命名规约

* **常量类**：以 `Consts` 或 `Constant` 结尾，如 `TemplateConstant`
* **常量**：全大写，单词间用下划线分隔，如 `MAX_RETRY_COUNT`
* **枚举类**：以枚举含义 + `Enum` 命名，如 `TaskStatusEnum`
* **枚举值**：全大写，单词间用下划线分隔，如 `PENDING`, `PROCESSING`, `COMPLETED`

### 2.6. 异常类命名规约

* 业务异常以 `Exception` 结尾，如 `TemplateTaskNotFoundException`
* 错误码常量类以 `ErrorCode` 结尾，如 `TemplateErrorCode`
* 建议按模块划分错误码范围，如模板模块使用 `10001-10999`

### 2.7. 包命名规范

* 包名统一使用**小写**，单词间不使用下划线或连字符
* 包名使用**单数**名词，禁止使用复数形式
  * 正例：`com.example.my.template.adapter.controller`
  * 反例：`com.example.my.template.adapters.controllers`
* 包名结构：`com.{公司}.{项目}.{模块}.{分层}`
  * 示例：`com.example.my.template.domain.gateway`

### 2.8. Converter 工具类命名规范

* 类名以转换目标类型的名称开头，如 `TemplateTaskDOConverter`
* 命名格式：`{源类型}To{目标类型}Converter` 或 `{目标类型}Converter`

---

## 3. 资源文件规范

### 3.1. 目录结构

```
src/main/resources/
├── META-INF/
│   └── spring.factories          # 自动装配配置
├── mapper/                       # MyBatis XML 映射文件
│   └── TemplateTaskMapper.xml
├── config/                       # 环境配置文件
│   ├── application-template.yml  # 模块专属配置
│   └── logback-spring.xml        # 日志配置（如有特殊需求）
├── i18n/                         # 国际化资源（可选）
│   └── messages.properties
└── scripts/                      # 数据库脚本（可选）
    └── V1.0.0__init_template.sql
```

### 3.2. 配置文件命名

* 模块专属配置：`application-{模块名}.yml`
* 配置项前缀：`{模块名}.{功能}`，如 `template.task.timeout=30`

---

## 4. 测试规范

### 4.1. 测试目录结构

```
src/test/java/com/example/my/template/
├── app/                          # 应用层测试
│   └── TemplateTaskServiceTest.java
├── domain/                       # 领域层单元测试
│   └── TemplateTaskTest.java
├── infrastructure/               # 基础设施层测试
│   └── TemplateTaskGatewayImplTest.java
└── adapter/                      # 适配层测试（如 Controller）
    └── TemplateTaskControllerTest.java
```

### 4.2. 测试类命名

* 单元测试：被测类名 + `Test`，如 `TemplateTaskServiceTest`
* 集成测试：被测类名 + `IT`，如 `TemplateTaskServiceIT`

---

## 5. 架构规范

### 5.1. 分层调用规则

| 层级 | 可调用层级 | 禁止调用 |
|------|-----------|----------|
| `adapter` | `app` | `domain`, `infrastructure` |
| `app` | `domain`, `infrastructure`（仅 gateway 接口） | 直接调用 repository/mapper |
| `domain` | 仅自身 | 任何其他层 |
| `infrastructure` | `domain`（实现 gateway） | `app`, `adapter` |

**核心原则**：
* 依赖关系**单向**，外层可以调用内层，内层不能反向依赖外层
* `domain` 层**最纯粹**，只包含业务逻辑，不依赖任何框架
* `app` 层负责**编排**领域逻辑，处理事务边界

### 5.2. 事务管理规范

* 事务注解 `@Transactional` 统一放在 **app 层** 的 Service 实现类上
* 禁止在 `adapter` 层（Controller）开启事务
* 禁止在 `domain` 层开启事务（保持领域纯净）

示例：
```java
@Service
public class TemplateTaskServiceImpl implements TemplateTaskService {
    
    @Transactional(rollbackFor = Exception.class)
    @Override
    public void createTask(CreateTemplateTaskCmd cmd) {
        // 业务编排
    }
}
```

### 5.3. 依赖注入规范

* **强制使用构造器注入**，禁止字段注入
* 优点：依赖明确、便于单元测试、避免循环依赖

示例：
```java
@Service
public class TemplateTaskServiceImpl implements TemplateTaskService {
    
    private final TemplateTaskGateway templateTaskGateway;
    private final TemplateTaskConverter templateTaskConverter;
    
    public TemplateTaskServiceImpl(TemplateTaskGateway templateTaskGateway,
                                   TemplateTaskConverter templateTaskConverter) {
        this.templateTaskGateway = templateTaskGateway;
        this.templateTaskConverter = templateTaskConverter;
    }
}
```

---

## 6. 日志规范

### 6.1. 日志使用

* 使用 SLF4J + Logback 组合
* 声明方式：使用 Lombok 的 `@Slf4j` 注解
  ```java
  @Slf4j
  @Service
  public class TemplateTaskServiceImpl implements TemplateTaskService {
      public void doSomething() {
          log.info("执行操作成功");
      }
  }
  ```
* 禁止使用 `System.out.println()` 输出日志

### 6.2. 日志级别

* **ERROR**：系统异常、需要立即处理的错误
* **WARN**：业务异常、可容忍的错误（如参数校验失败）
* **INFO**：关键业务流程节点、状态变更
* **DEBUG**：详细调试信息，生产环境关闭

### 6.3. 日志内容规范

* 关键日志必须包含**业务标识**（如 taskId、userId）
* 示例：`log.info("创建模板任务成功, taskId={}, operator={}", taskId, userId);`

---

## 7. 代码注释规范

### 7.1. 类注释

```java
/**
 * 模板任务应用服务
 * 
 * @author {作者}
 * @since {版本号}
 */
public class TemplateTaskServiceImpl implements TemplateTaskService {
```

### 7.2. 方法注释

```java
/**
 * 创建模板任务
 *
 * @param cmd 创建命令
 * @return 任务ID
 * @throws TemplateTaskException 当任务创建失败时抛出
 */
Long createTask(CreateTemplateTaskCmd cmd);
```

### 7.3. 代码块注释

* 复杂业务逻辑需添加行内注释说明意图
* 使用 `//` 而非 `/* */`，保持简洁

---

## 8. 数据库规范

### 8.1. 表命名

* 统一使用小写，单词间用下划线分隔
* 模块前缀 + 表名，如 `template_task`, `template_task_log`
* 禁止使用复数形式

### 8.2. 字段命名

* 主键统一命名为 `id`
* 创建时间：`created_at`，更新时间：`updated_at`
* 逻辑删除标记：`is_deleted`（0-未删除，1-已删除）
* 状态字段：以 `status` 结尾，如 `task_status`

### 8.3. 索引命名

* 主键：`PRIMARY`
* 唯一索引：`uk_{字段名}`，如 `uk_task_code`
* 普通索引：`idx_{字段名}`，如 `idx_created_at`


