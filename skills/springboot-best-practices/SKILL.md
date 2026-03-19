---
name: springboot-best-practices
description: |
  Spring Boot 项目开发规范，包含 Java 命名规范、JavaDoc 注释规范、模块划分规范、pom.xml 规范、Spring Boot 自动装配规范。
  触发场景：(1) 创建 Spring Boot 项目或模块 (2) 编写 Java 代码 (3) 设计模块结构 (4) 配置自动装配 (5) 管理 Maven 依赖
---

# Spring Boot Best Practices

## 概述

Spring Boot 项目开发的通用规范，涵盖命名、注释、模块设计、依赖管理、自动装配。

## 快速参考

### 命名规范

| 类型 | 规则 | 示例 |
|-----|------|------|
| 类名 | UpperCamelCase | `UserService` |
| 方法/变量 | lowerCamelCase | `getUserById()` |
| 常量 | UPPER_SNAKE_CASE | `MAX_SIZE` |
| 包名 | 小写 | `com.example.service` |

### JavaDoc 要求

- 公开类/接口：必须有 JavaDoc
- 公开方法：必须有 JavaDoc，包含 `@param`、`@return`
- 抛异常方法：必须有 `@throws`
- 不使用 `@author`、`@version`

### 模块划分

- **普通模块**：`{project}-{domain}`
- **多实现模块**：二级父模块 + 三级抽象 + 三级实现
  ```
  {project}-cache/
  ├── {project}-cache-api/
  ├── {project}-cache-jvm/
  └── {project}-cache-redis/
  ```

### pom.xml

- 继承 `spring-boot-starter-parent`
- 版本集中在父 POM 的 `dependencyManagement`
- 子模块引用不指定版本

### 自动装配

- 使用 `AutoConfiguration.imports` 注册
- `@ConfigurationProperties` 绑定配置
- `@ConditionalOnProperty` / `@ConditionalOnMissingBean` 控制生效

## 详细文档

按需查阅以下文件：

- [命名规范](references/naming.md)
- [JavaDoc 注释规范](references/javadoc.md)
- [模块划分规范](references/module-design.md)
- [pom.xml 规范](references/pomxml.md)
- [Spring Boot 自动装配规范](references/auto-configuration.md)
