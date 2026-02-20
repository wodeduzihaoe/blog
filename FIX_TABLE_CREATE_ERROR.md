# 修复创建表时的错误

## ❌ 错误信息

```
ERROR 1067 (42000) at line 1: Invalid default value for 'id'
```

## 🔧 问题原因

`AUTO_INCREMENT` **不能**作为default值。在MySQL中，`AUTO_INCREMENT`是一个列属性，不是默认值。

## ✅ 修复方法

### 修改id列的配置：

1. **找到id列**
2. **修改Default字段**：
   - 将 `AUTO_INCREMENT` 改为 `no default` 或留空
3. **确保Constraints中有**：
   - `Primary Key` ✓
   - `AUTO_INCREMENT` 应该通过其他方式设置（可能在type或constraints中）

### 具体操作：

**id列的正确配置**：
- column name: `id`
- type: `BIGINT`
- **default: `no default`** ← 改为这个！
- constraints: `Primary Key` 和 `AUTO_INCREMENT`（如果界面支持）

**如果界面不支持AUTO_INCREMENT约束**：
- 先创建表，然后使用SQL修改：
  ```sql
  ALTER TABLE users MODIFY id BIGINT AUTO_INCREMENT;
  ```

---

## 📋 完整的列配置（修正版）

### 列1 - id：
- column name: `id`
- type: `BIGINT`
- **default: `no default`** ← 重要！
- constraints: `Primary Key`

### 列2 - username：
- column name: `username`
- type: `VARCHAR(50)`
- default: `no default`
- constraints: `UNIQUE` 和 `Not NULL`

### 列3 - password：
- column name: `password`
- type: `VARCHAR(255)`
- default: `no default`
- constraints: `Not NULL`

### 列4 - email：
- column name: `email`
- type: `VARCHAR(100)`
- default: `no default`
- constraints: 不勾选（可选字段）

### 列5 - create_time：
- column name: `create_time`
- type: `DATETIME`
- default: `CURRENT_TIMESTAMP`
- constraints: `Not NULL`

### 列6 - update_time：
- column name: `update_time`
- type: `DATETIME`
- default: `CURRENT_TIMESTAMP` 或 `no default`
- constraints: `Not NULL`

---

## 🎯 操作步骤

1. **修改id列的default**：从 `AUTO_INCREMENT` 改为 `no default`
2. **确保id列有Primary Key约束**
3. **其他列保持不变**
4. **点击 "Create" 按钮**

---

## ⚠️ 如果创建后id没有AUTO_INCREMENT

如果表创建成功但id列没有自动递增，可以：

1. **查找SQL查询界面**（在Database标签中）
2. **执行以下SQL**：
   ```sql
   ALTER TABLE users MODIFY id BIGINT AUTO_INCREMENT;
   ```

---

## 🔄 或者：直接使用SQL创建（推荐）

如果图形界面太复杂，建议：

1. **查找 "Query" 或 "SQL Console" 按钮**
2. **直接执行SQL**：
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

这样更简单，不会出错！

