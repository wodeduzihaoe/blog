# ⚡ 快速开始 - 免费部署指南

这是最简化的部署步骤，按照顺序执行即可。

## 📝 前提条件

- [x] 有GitHub账号（如果没有，去 https://github.com 注册）
- [x] 项目代码已准备好

---

## 🎯 5步完成部署

### 步骤1️⃣：上传代码到GitHub（5分钟）

```bash
# 在项目根目录执行
cd C:\Users\yixian\Desktop\blog

# 如果还没有初始化Git
git init
git add .
git commit -m "Initial commit"

# 在GitHub创建新仓库，然后执行（替换your-username）
git remote add origin https://github.com/your-username/blog.git
git branch -M main
git push -u origin main
```

**不会用Git？** 看这里：
1. 下载Git：https://git-scm.com/download/win
2. 安装后，在项目文件夹右键 → "Git Bash Here"
3. 执行上面的命令

---

### 步骤2️⃣：创建数据库（PlanetScale）（10分钟）

1. **注册账号**
   - 访问：https://planetscale.com/
   - 点击 "Sign up" → 使用GitHub登录

2. **创建数据库**
   - 点击 "Create database"
   - 名称：`blog_db`
   - Plan：选择 `Free`
   - 点击 "Create"

3. **获取连接信息**
   - 点击 "Connect" → "General"
   - 复制：Host、Username、Password（点击Show password）

4. **创建数据表**
   - 点击 "Console" → "New branch" → 输入 `main`
   - 在SQL编辑器中粘贴：

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

   - 点击 "Apply"
   - 点击 "Create deploy request" → "Deploy"

**✅ 完成！记录下连接信息，下一步要用**

---

### 步骤3️⃣：部署后端（Railway）（15分钟）

1. **注册账号**
   - 访问：https://railway.app/
   - 点击 "Start a New Project" → 使用GitHub登录

2. **创建项目**
   - 点击 "New Project"
   - 选择 "Deploy from GitHub repo"
   - 选择你的 `blog` 仓库
   - **重要**：在 "Root Directory" 中选择 `backend`

3. **配置环境变量**
   - 点击 "Variables" 标签
   - 添加以下变量（点击 "New Variable" 逐个添加）：

```
SPRING_DATASOURCE_URL=jdbc:mysql://你的PlanetScale-Host:3306/blog_db?useUnicode=true&characterEncoding=utf8&useSSL=true&requireSSL=true&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=你的PlanetScale用户名
SPRING_DATASOURCE_PASSWORD=你的PlanetScale密码
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
JWT_SECRET=随便写一个至少32位的随机字符串，比如：my-super-secret-jwt-key-2024-change-this
JWT_EXPIRATION=86400000
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=prod
```

   **注意**：把 `你的PlanetScale-Host`、`你的PlanetScale用户名`、`你的PlanetScale密码` 替换为步骤2中复制的信息

4. **等待部署**
   - Railway会自动开始构建和部署
   - 等待3-5分钟
   - 部署成功后，会显示一个URL，例如：`https://xxx.railway.app`
   - **✅ 记录这个URL，这是你的后端地址！**

5. **测试后端**
   - 在浏览器访问：`https://xxx.railway.app/api/auth/user`
   - 如果看到错误信息（这是正常的），说明后端部署成功！

---

### 步骤4️⃣：部署前端（Vercel）（10分钟）

1. **注册账号**
   - 访问：https://vercel.com/
   - 点击 "Sign Up" → 使用GitHub登录

2. **创建项目**
   - 点击 "Add New..." → "Project"
   - 选择你的 `blog` 仓库
   - 配置：
     - **Framework Preset**: `Vite`
     - **Root Directory**: `frontend`（点击Edit，选择frontend文件夹）
     - **Build Command**: `npm run build`（自动填充）
     - **Output Directory**: `dist`（自动填充）

3. **添加环境变量**
   - 在 "Environment Variables" 部分
   - 点击 "Add"
   - Key: `VITE_API_BASE_URL`
   - Value: `https://xxx.railway.app/api`（替换为步骤3中记录的Railway后端地址）
   - 勾选：Production、Preview、Development

4. **部署**
   - 点击 "Deploy"
   - 等待2-3分钟
   - 部署完成后会显示URL，例如：`https://blog-xxx.vercel.app`

**✅ 完成！这就是你的网站地址！**

---

### 步骤5️⃣：测试网站（5分钟）

1. **访问网站**
   - 打开Vercel提供的URL
   - 应该能看到登录/注册页面

2. **测试功能**
   - 点击 "注册" → 填写信息 → 注册
   - 如果成功，会自动跳转到首页
   - 退出登录，再测试登录功能

3. **检查错误**
   - 按F12打开开发者工具
   - 查看Console标签是否有错误
   - 查看Network标签，API请求是否成功

**✅ 如果一切正常，恭喜你部署成功！🎉**

---

## 🎉 完成！

你的博客网站现在已经完全免费地部署在云端了！

**网站地址：** `https://xxx.vercel.app`

---

## ❓ 遇到问题？

### 问题1：后端部署失败

**检查：**
- Railway日志（点击Deployments → 查看日志）
- 环境变量是否正确
- 数据库连接信息是否正确

### 问题2：前端无法连接后端

**检查：**
- Vercel环境变量 `VITE_API_BASE_URL` 是否正确
- 后端URL是否以 `/api` 结尾
- 浏览器控制台是否有CORS错误

### 问题3：注册/登录失败

**检查：**
- 数据库表是否创建成功
- 后端日志是否有错误
- 网络请求是否正常（F12 → Network）

---

## 📚 详细文档

- 完整部署指南：`FREE_DEPLOY.md`
- 环境变量配置：`ENV_CONFIG.md`
- 部署检查清单：`DEPLOY_CHECKLIST.md`

---

## 🔄 更新代码

以后修改代码后，只需要：

```bash
git add .
git commit -m "Update code"
git push origin main
```

Railway和Vercel会自动重新部署！

---

**需要帮助？** 查看详细文档或检查日志！

