# 在Railway中创建数据表的两种方法

## 方法一：使用图形化界面创建表（当前界面）

你看到的 "Create table" 界面是图形化的，可以这样创建：

### 步骤：

1. **表名**：将 `my-table` 改为 `users`

2. **删除默认的id列**（如果存在）：
   - 点击id列旁边的删除按钮

3. **添加列**，点击 "Add column" 逐个添加：

   **第1列 - id**：
   - column name: `id`
   - type: `BIGINT`（不是serial，MySQL用BIGINT）
   - default: `AUTO_INCREMENT` 或留空
   - constraints: `Primary Key`

   **第2列 - username**：
   - column name: `username`
   - type: `VARCHAR(50)`
   - default: `no default`
   - constraints: `UNIQUE` 和 `NOT NULL`

   **第3列 - password**：
   - column name: `password`
   - type: `VARCHAR(255)`
   - default: `no default`
   - constraints: `NOT NULL`

   **第4列 - email**：
   - column name: `email`
   - type: `VARCHAR(100)`
   - default: `no default`
   - constraints: 留空（可选）

   **第5列 - create_time**：
   - column name: `create_time`
   - type: `DATETIME`
   - default: `CURRENT_TIMESTAMP` 或 `no default`
   - constraints: `NOT NULL`

   **第6列 - update_time**：
   - column name: `update_time`
   - type: `DATETIME`
   - default: `CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP` 或 `no default`
   - constraints: `NOT NULL`

4. **点击 "Create" 按钮**创建表

---

## 方法二：使用SQL查询界面（如果可用）

### 查找SQL查询入口：

1. **在Database标签中查找**：
   - 查找 "Query"、"SQL"、"Console" 或 "Run SQL" 按钮
   - 可能在 "Data" 标签旁边，或者在页面其他地方

2. **或者使用Connect按钮**：
   - 点击 "Connect" 按钮
   - 可能会显示连接字符串，也可能有查询界面

3. **如果找到SQL界面**，直接执行：

```sql
CREATE TABLE IF NOT EXISTS users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(100),
    create_time DATETIME NOT NULL,
    update_time DATETIME NOT NULL,
    INDEX idx_username (username),
    INDEX idx_email (email)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

---

## 方法三：使用外部MySQL客户端连接

如果Railway没有SQL查询界面，可以使用外部工具：

### 使用MySQL Workbench或其他客户端：

1. **获取连接信息**（从Variables标签）：
   - 使用 `MYSQL_PUBLIC_URL` 或手动组合：
     - Host: `switchback.proxy.rlwy.net`
     - Port: `34066`
     - User: `root`
     - Password: `spIFevsdIsgEfwCEPWCfnY0jTI1YPDOI`
     - Database: `railway`

2. **连接后执行SQL**：
   ```sql
   CREATE TABLE IF NOT EXISTS users (
       id BIGINT AUTO_INCREMENT PRIMARY KEY,
       username VARCHAR(50) NOT NULL UNIQUE,
       password VARCHAR(255) NOT NULL,
       email VARCHAR(100),
       create_time DATETIME NOT NULL,
       update_time DATETIME NOT NULL,
       INDEX idx_username (username),
       INDEX idx_email (email)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
   ```

---

## 🎯 推荐操作

**最简单的方法**：使用图形化界面（方法一）

1. 表名改为 `users`
2. 删除默认列
3. 添加6个列（按照上面的配置）
4. 点击 "Create"

**或者**：查找 "Query" 或 "SQL" 按钮，使用SQL直接创建（更快）

---

## ⚠️ 注意事项

1. **MySQL类型**：确保使用 `BIGINT` 而不是 `serial`（serial是PostgreSQL的类型）
2. **AUTO_INCREMENT**：id列需要设置为自动递增
3. **字符集**：如果图形界面有选项，选择 `utf8mb4`

---

## 📝 如果图形界面太复杂

如果图形界面操作复杂，建议：
1. 先查找是否有 "Query" 或 "SQL Console" 按钮
2. 如果没有，使用外部MySQL客户端连接
3. 或者告诉我，我可以帮你找其他方法

