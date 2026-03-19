# pom.xml 规范

## 依赖版本管理

### 父 POM 职责

| 职责 | 说明 |
|-----|------|
| 继承 starter-parent | 统一依赖版本管理 |
| dependencyManagement | 仅在此处管理第三方依赖版本 |
| properties | 定义版本属性，便于维护 |

### 子模块职责

- 直接引用依赖
- 不指定版本（除非 starter-parent 已管理）

## 示例

### 父 pom.xml

```xml
<project>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.4.0</version>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>parent</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <properties>
        <java.version>17</java.version>
        <lark.version>2.5.3</lark.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.lark.oapi</groupId>
                <artifactId>lark-oapi</artifactId>
                <version>${lark.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

### 子模块 pom.xml

```xml
<project>
    <parent>
        <groupId>com.example</groupId>
        <artifactId>parent</artifactId>
    </parent>

    <artifactId>user-service</artifactId>

    <dependencies>
        <!-- 不指定版本，继承父 POM -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 不指定版本，在 dependencyManagement 中定义 -->
        <dependency>
            <groupId>com.lark.oapi</groupId>
            <artifactId>lark-oapi</artifactId>
        </dependency>
    </dependencies>
</project>
```

## 常见问题

| 问题 | 解决 |
|-----|------|
| 子模块报错找不到版本 | 确认父 POM 已 在 dependencyManagement 中定义 |
| 版本不一致 | 使用 properties 统一管理版本 |
