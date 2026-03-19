# 模块划分规范

## 原则

| 原则 | 说明 |
|-----|------|
| 单一职责 | 每个模块只负责一件事 |
| 稳定抽象 | 抽象模块不依赖具体实现 |
| 依赖单向 | 业务模块可依赖基础设施，基础设施不依赖业务 |
| 可替换性 | 多实现场景通过接口抽象实现替换 |

## 普通单模块

```
{project}-web/          # 主模块（应用入口）
{project}-user/         # 业务模块（用户服务）
{project}-order/        # 业务模块（订单服务）
```

## 多实现模块

当某个功能可能有多种实现时，使用二级父模块 + 三级子模块的结构。

### 结构示意

```
{project}-cache/                    # 二级父模块（POM 类型）
├── {project}-cache-api/             # 三级抽象模块（接口定义）
├── {project}-cache-jvm/             # 三级实现（JVM 内存）
└── {project}-cache-redis/          # 三级实现（Redis）
```

### 设计目的

当需要替换实现时（如 Redis 换 Memcached），只需：
1. 新增实现模块
2. 修改主模块依赖

业务代码无需改动。

### 典型场景示例

| 场景 | 二级父模块 | 抽象模块 | 实现模块 |
|-----|-----------|---------|---------|
| 缓存 | `{project}-cache` | `{project}-cache-api` | `{project}-cache-jvm`, `{project}-cache-redis` |
| 消息队列 | `{project}-mq` | `{project}-mq-api` | `{project}-mq-kafka`, `{project}-mq-rabbit` |
| 文件存储 | `{project}-storage` | `{project}-storage-api` | `{project}-storage-local`, `{project}-storage-oss` |
| HTTP 客户端 | `{project}-http` | `{project}-http-api` | `{project}-http-okhttp`, `{project}-http-apache` |

### 模块依赖关系

```
主模块
├── 业务模块 A
│   └── 抽象模块 X
│       └── 实现模块 X-a (运行时依赖)
├── 业务模块 B
└── 基础设施模块
```

业务模块依赖抽象接口，不直接依赖实现模块。
