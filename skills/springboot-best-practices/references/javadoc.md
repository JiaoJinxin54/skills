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

### 属性注释

公开属性必须添加 JavaDoc，私有属性可选。

```java
public class User {
    /**
     * 用户 ID
     */
    private Long id;

    /**
     * 用户名
     */
    private String username;

    /**
     * 状态：0-禁用，1-启用
     */
    private Integer status;
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

### 接口与实现类注释规则

当接口与其实现类同时存在时，方法 JavaDoc 仅写在接口中，实现类中的方法不需要重复添加注释。实现类的方法签名应与接口保持一致。

```java
// ✅ 正确：注释写在接口上
public interface UserService {
    /**
     * 根据 ID 获取用户
     * @param id 用户 ID
     * @return 用户对象，不存在返回 null
     * @throws IllegalArgumentException 如果 ID 为空
     */
    User getUserById(Long id);
}

public class UserServiceImpl implements UserService {
    // 无需再给 getUserById 方法添加 JavaDoc
    @Override
    public User getUserById(Long id) {
        // ...
    }
}
```

```java
// ❌ 错误：实现类不应重复注释
public class UserServiceImpl implements UserService {
    /**
     * 根据 ID 获取用户
     * @param id 用户 ID
     * @return 用户对象，不存在返回 null
     */
    @Override
    public User getUserById(Long id) {
        // ...
    }
}
```

**说明**：JavaDoc 的主要作用是生成 API 文档，当接口已经提供了完整的文档说明，实现类的重复注释只会增加维护成本。

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

| 风格 | 适用场景 | 禁忌 |
|-----|---------|-----|
| JavaDoc `/** */` | 公开 API、类说明、方法说明、属性说明 | 禁止行尾 JavaDoc |
| 行注释 `// comment` | 行内说明、临时注释 | 禁止行尾注释 |
| 块注释 `/* comment */` | 仅用于禁用代码 | - |

## 禁止的反向要求

### 行尾注释禁止

```java
// ❌ 错误：行尾注释
private String name; // 用户姓名

// ✅ 正确：单独行注释
private String name;
// 用户姓名
```

### 注释与代码同行禁止

```java
// ❌ 错误：if 语句同行注释
if (isValid) { // 检查参数有效性

// ✅ 正确：注释单独成行
// 检查参数有效性
if (isValid) {
```

### 多余的描述性注释禁止

```java
// ❌ 错误：废话注释
// 设置用户名
user.setUsername(name);

// ✅ 正确：无需注释，代码即说明
user.setUsername(name);

// ✅ 如果需要注释，说明"为什么"而非"做什么"
// 使用setUsername因为需要触发验证器
user.setUsername(name);
```

## 注意事项

- 不要在 JavaDoc 中添加 `@author` 或 `@version`
- 保持注释简洁，描述清晰
- 同步更新已废弃代码的 JavaDoc
- 接口与实现类共存时，方法注释仅写在接口上，实现类无需重复
