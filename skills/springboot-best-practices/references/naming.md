# 命名规范

## Java 命名

| 类型 | 规则 | 示例 |
|-----|------|------|
| 类名、接口名 | UpperCamelCase | `UserService`, `OrderController` |
| 方法名、变量名 | lowerCamelCase | `getUserById()`, `orderList` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `DEFAULT_PAGE_SIZE` |
| 包名 | 全部小写 | `com.example.service` |

## Maven 模块命名

- 使用小写字母和连字符
- 格式：`{project}-{domain}` 或 `{project}-{domain}-{impl}`

## 配置属性命名

- 使用小写字母和连字符
- 示例：`spring.application.name`, `lark.app-id`

## 示例

```java
// 类名 - UpperCamelCase
public class UserServiceImpl {

    // 常量 - UPPER_SNAKE_CASE
    private static final int MAX_RETRY_COUNT = 3;

    // 变量 - lowerCamelCase
    private UserRepository userRepository;

    // 方法 - lowerCamelCase
    public User getUserById(Long id) {
        return userRepository.findById(id);
    }
}
```
