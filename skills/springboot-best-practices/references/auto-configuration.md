# Spring Boot 自动装配规范

## 配置注册（Spring Boot 3.0+）

使用 `AutoConfiguration.imports` 文件注册自动配置。

### 文件位置

```
src/main/resources/META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
```

### 文件内容

```
com.example.cache.RedisAutoConfiguration
com.example.cache.JvmCacheAutoConfiguration
```

## 配置属性类

### 规范

| 注解 | 用途 |
|-----|------|
| `@ConfigurationProperties` | 绑定配置前缀 |
| `@EnableConfigurationProperties` | 启用属性配置类 |
| `@AutoConfiguration` | 标记为自动配置类 |

### 示例

```java
@ConfigurationProperties("cache")
public class CacheProperties {
    private boolean enabled = true;
    private int maxSize = 1000;
}
```

```java
@AutoConfiguration
@EnableConfigurationProperties(CacheProperties.class)
public class CacheAutoConfiguration {
    @Bean
    @ConditionalOnMissingBean
    public CacheService cacheService(CacheProperties properties) {
        return new JvmCacheService(properties);
    }
}
```

## 条件装配

| 注解 | 用途 |
|-----|------|
| `@ConditionalOnProperty` | 配置项满足条件时生效 |
| `@ConditionalOnMissingBean` | Bean 不存在时创建 |
| `@ConditionalOnClass` | 类路径存在时生效 |

### 示例

```java
// 类级别：只有配置了 cache.enabled=true 才生效
@AutoConfiguration
@ConditionalOnProperty(prefix = "cache", name = "enabled", havingValue = "true")
public class CacheAutoConfiguration {
}

// 方法级别：用户未自定义 CacheService 时使用默认
@Bean
@ConditionalOnMissingBean
public CacheService cacheService() {
    return new DefaultCacheService();
}
```

## 环境配置

### 环境变量注入

```yaml
lark:
  app-id: ${LARK_APP_ID:}           # 必填，无默认值
  app-secret: ${LARK_APP_SECRET:}   # 必填，无默认值
  debug: ${DEBUG:false}             # 可选，有默认值
```

### 配置前缀规范

| 前缀 | 用途 |
|-----|------|
| `spring.*` | Spring 标准配置 |
| `app.*` | 应用级配置 |
| `{provider}.*` | 第三方服务配置（如 `lark.*`） |

## 配置文件结构

```yaml
server:
  port: 8080

spring:
  application:
    name: application-name
  profiles:
    active: dev

# 第三方服务
lark:
  app-id: ${LARK_APP_ID:}
  app-secret: ${LARK_APP_SECRET:}

# 应用配置
cache:
  enabled: true
  type: redis

logging:
  level:
    com.example: DEBUG
```
