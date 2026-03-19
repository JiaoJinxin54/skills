# JavaDoc 注释规范

## 必须添加 JavaDoc 的场景

| 元素 | 要求 |
|-----|------|
| 公开类/接口 | 必须有 JavaDoc |
| 公开方法 | 必须有 JavaDoc，包含 `@param`、`@return` |
| 抛出异常的方法 | 必须有 `@throws` 声明 |

## JavaDoc 格式

### 类的注释

```java
/**
 * 用户服务实现类
 * 负责用户的 CRUD 操作
 */
public class UserServiceImpl {
}
```

### 方法的注释

```java
/**
 * 根据 ID 获取用户
 * @param id 用户 ID
 * @return 用户对象，不存在返回 null
 * @throws IllegalArgumentException 如果 ID 为空
 */
public User getUserById(Long id) {
}
```

### 废弃方法

```java
/**
 * @deprecated 已废弃，请使用 {@link #getUserById(Long)}
 */
@Deprecated
public User findUserById(Long id) {
}
```

## 注释风格选择

| 风格 | 适用场景 |
|-----|---------|
| JavaDoc `/** */` | 公开 API、类说明、方法说明 |
| 行注释 `// comment` | 行内说明、临时注释 |
| 块注释 `/* comment */` | 仅用于禁用代码 |

## 注意事项

- 不要在 JavaDoc 中添加 `@author` 或 `@version`
- 保持注释简洁，描述清晰
- 同步更新已废弃代码的 JavaDoc
