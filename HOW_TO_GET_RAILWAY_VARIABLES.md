# 如何获取Railway MySQL连接信息

## ⚠️ 重要说明

**这些变量不需要你填写！** Railway会自动生成这些值，你只需要：
1. 等待数据库创建完成
2. 查看这些变量的值
3. 复制保存，稍后配置后端时使用

---

## 📋 详细步骤

### 步骤1：等待数据库创建完成

1. 在Railway界面中，查看MySQL服务
2. 等待 "The MySQL database is being created..." 消息消失
3. 看到 "Database" 标签可以正常使用

**通常需要1-2分钟**

---

### 步骤2：查看Variables（环境变量）

1. **点击左侧的MySQL服务卡片**（绿色的MySQL卡片）
2. **在右侧面板，点击 "Variables" 标签**
3. **你会看到类似这样的列表**：

```
MYSQL_HOST=containers-us-xxx.railway.app
MYSQL_PORT=3306
MYSQLDATABASE=railway
MYSQLUSER=root
MYSQLPASSWORD=xxxxxxxxxxxxx
```

---

### 步骤3：复制这些值

**方法一：逐个复制**
1. 找到 `MYSQL_HOST`，复制它的值（例如：`containers-us-xxx.railway.app`）
2. 找到 `MYSQL_PORT`，复制它的值（通常是 `3306`）
3. 找到 `MYSQLDATABASE`，复制它的值（例如：`railway`）
4. 找到 `MYSQLUSER`，复制它的值（通常是 `root`）
5. 找到 `MYSQLPASSWORD`，点击眼睛图标👁️显示密码，然后复制

**方法二：一键复制所有**
- Railway可能提供 "Copy all" 或 "Export" 按钮
- 点击后可以复制所有变量

---

### 步骤4：保存这些信息

**建议保存到一个文本文件中**，例如：

```
Railway MySQL 连接信息：

MYSQL_HOST=containers-us-xxx.railway.app
MYSQL_PORT=3306
MYSQLDATABASE=railway
MYSQLUSER=root
MYSQLPASSWORD=your_password_here
```

**或者直接保存到 `RAILWAY_DB_INFO.txt` 文件中**

---

## 📝 示例

假设你看到的变量值是：

```
MYSQL_HOST=containers-us-123.railway.app
MYSQL_PORT=3306
MYSQLDATABASE=railway
MYSQLUSER=root
MYSQLPASSWORD=abc123xyz456
```

那么你需要保存的就是：
- **MYSQL_HOST**: `containers-us-123.railway.app`
- **MYSQL_PORT**: `3306`
- **MYSQLDATABASE**: `railway`
- **MYSQLUSER**: `root`
- **MYSQLPASSWORD**: `abc123xyz456`

---

## 🔍 如果找不到Variables标签？

1. **确保点击了MySQL服务卡片**（左侧的绿色卡片）
2. **查看右侧面板的标签栏**，应该有：
   - Deployments
   - Database
   - Backups
   - **Variables** ← 点击这个
   - Metrics
   - Settings

3. **如果还是没有**，可能是数据库还在创建中，请等待完成

---

## ⚠️ 重要提示

1. **密码只显示一次**：MYSQLPASSWORD可能需要点击眼睛图标才能看到
2. **立即保存**：复制后立即保存，避免丢失
3. **不要分享**：这些是敏感信息，不要分享给他人

---

## 🎯 下一步

获取到这些信息后，下一步是：
1. 在Railway中创建后端服务
2. 配置环境变量（使用这些数据库连接信息）
3. 部署后端

按照 `QUICK_START.md` 的步骤3继续操作即可！

