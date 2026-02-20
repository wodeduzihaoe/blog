# Railway部署后端 - 不需要GitHub授权的方法

## 🎯 解决方案

由于GitHub账号被标记，无法授权Railway连接GitHub仓库。有以下几种解决方案：

---

## 方案一：使用新GitHub账号（推荐，如果可行）

### 优点
- ✅ 最简单，保持原有工作流程
- ✅ 可以继续使用GitHub
- ✅ Railway可以自动部署（代码推送后自动更新）

### 缺点
- ❌ 需要创建新账号
- ❌ 需要重新推送代码到新账号

### 操作步骤

1. **创建新GitHub账号**
   - 使用新邮箱注册GitHub账号
   - 完成邮箱验证

2. **推送代码到新账号**
   ```bash
   # 添加新远程仓库
   git remote add new-origin https://github.com/new-username/blog.git
   
   # 推送代码
   git push new-origin main
   ```

3. **在Railway中连接新账号的仓库**
   - 使用新GitHub账号授权Railway
   - 选择新账号的仓库

---

## 方案二：使用Railway CLI直接部署（不需要GitHub）

### 优点
- ✅ 不需要GitHub授权
- ✅ 直接从本地代码部署
- ✅ 完全控制部署过程

### 缺点
- ❌ 需要安装CLI工具
- ❌ 代码更新需要手动部署

### 操作步骤

1. **安装Railway CLI**
   ```bash
   # Windows (使用PowerShell)
   irm https://railway.app/install.ps1 | iex
   
   # 或使用npm
   npm install -g @railway/cli
   ```

2. **登录Railway**
   ```bash
   railway login
   ```

3. **在项目目录中初始化**
   ```bash
   cd C:\Users\yixian\Desktop\blog\backend
   railway init
   ```

4. **部署**
   ```bash
   railway up
   ```

5. **配置环境变量**
   ```bash
   railway variables set SPRING_DATASOURCE_URL="jdbc:mysql://mysql.railway.internal:3306/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai"
   railway variables set SPRING_DATASOURCE_USERNAME="root"
   railway variables set SPRING_DATASOURCE_PASSWORD="your_password"
   # ... 其他变量
   ```

---

## 方案三：使用Docker部署（推荐，灵活）

### 优点
- ✅ 不需要GitHub授权
- ✅ 可以手动上传代码
- ✅ 更灵活

### 操作步骤

1. **在Railway中创建Empty Service**
   - 点击 "+ New" → "Empty Service"

2. **上传代码**
   - 方式A：使用Railway CLI（见方案二）
   - 方式B：使用Git（但需要解决授权问题）

3. **配置构建和启动命令**
   - Build Command: `cd backend && mvn clean package -DskipTests`
   - Start Command: `cd backend && java -jar target/blog-backend-1.0.0.jar`

---

## 方案四：使用其他平台部署后端

如果Railway的GitHub授权问题无法解决，可以考虑：

### Render
- 访问：https://render.com/
- 支持邮箱注册
- 可以连接GitHub（但可能也有同样问题）
- 或使用Render CLI部署

### Fly.io
- 访问：https://fly.io/
- 支持邮箱注册
- 使用flyctl CLI部署

---

## 🎯 我的推荐

### 如果方便创建新GitHub账号：
**使用方案一** - 创建新GitHub账号，最简单

### 如果不想创建新账号：
**使用方案二** - Railway CLI直接部署，不需要GitHub授权

---

## 📝 快速开始（方案二 - Railway CLI）

如果你想使用CLI部署，我可以帮你：

1. 安装Railway CLI
2. 登录Railway
3. 部署后端
4. 配置环境变量

告诉我你想用哪个方案，我帮你详细操作！

