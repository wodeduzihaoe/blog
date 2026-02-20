# 🆓 完全免费部署方案

本指南将帮你**完全免费**地将博客网站部署到云端。

## 📦 部署架构

```
前端 (Vercel) → 后端 (Railway) → 数据库 (PlanetScale)
  免费CDN         免费托管         免费MySQL
```

**总成本：$0（完全免费）**

---

## 🎯 方案概览

| 服务 | 平台 | 免费额度 | 说明 |
|------|------|----------|------|
| **前端** | Vercel | 无限 | 全球CDN，自动HTTPS |
| **后端** | Railway | $5/月免费额度 | 足够小项目使用 |
| **数据库** | PlanetScale | 5GB免费 | MySQL兼容，性能好 |

---

## 📋 部署步骤总览

1. ✅ 准备GitHub仓库
2. ✅ 部署数据库（PlanetScale）
3. ✅ 部署后端（Railway）
4. ✅ 部署前端（Vercel）
5. ✅ 配置域名（可选）

---

## 第一步：准备GitHub仓库

### 1.1 创建GitHub仓库

1. 访问 [GitHub](https://github.com)
2. 点击右上角 "+" → "New repository"
3. 仓库名：`blog`（或你喜欢的名字）
4. 选择 Public（免费方案需要公开仓库）
5. 点击 "Create repository"

### 1.2 上传代码到GitHub

```bash
# 在项目根目录执行
cd C:\Users\yixian\Desktop\blog

# 初始化Git（如果还没有）
git init

# 添加所有文件
git add .

# 提交
git commit -m "Initial commit"

# 添加远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/your-username/blog.git

# 推送代码
git branch -M main
git push -u origin main
```

**如果遇到问题：**
- 需要先安装Git：https://git-scm.com/download/win
- 需要配置Git用户信息：
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```

---

## 第二步：部署数据库（PlanetScale）

### 2.1 注册PlanetScale账号

1. 访问 [PlanetScale](https://planetscale.com/)
2. 点击 "Sign up" 使用GitHub账号登录（推荐）
3. 完成注册

### 2.2 创建数据库

1. 登录后，点击 "Create database"
2. 填写信息：
   - **Database name**: `blog_db`
   - **Region**: 选择离你最近的（如 `ap-southeast-1`）
   - **Plan**: 选择 `Free`
3. 点击 "Create database"

### 2.3 获取数据库连接信息

1. 在数据库页面，点击 "Connect"
2. 选择 "General" 标签
3. 复制以下信息（稍后会用到）：
   - **Host**: `xxx.psdb.cloud`
   - **Username**: `xxx`
   - **Password**: `xxx`（点击 "Show password"）
   - **Database**: `blog_db`
   - **Port**: `3306`

### 2.4 导入数据库结构

1. 在PlanetScale控制台，点击 "Console" 标签
2. 点击 "New branch" → 输入分支名 `main`
3. 在SQL编辑器中，粘贴以下SQL（或从 `backend/src/main/resources/db/schema.sql` 复制）：

```sql
CREATE TABLE IF NOT EXISTS users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

4. 点击 "Apply" 执行
5. 点击 "Create deploy request" → "Deploy" 部署到生产环境

---

## 第三步：部署后端（Railway）

### 3.1 注册Railway账号

1. 访问 [Railway](https://railway.app/)
2. 点击 "Start a New Project"
3. 使用GitHub账号登录
4. 授权Railway访问你的GitHub仓库

### 3.2 创建新项目

1. 点击 "New Project"
2. 选择 "Deploy from GitHub repo"
3. 选择你的 `blog` 仓库
4. 在 "Root Directory" 中选择 `backend` 目录

### 3.3 配置环境变量

在Railway项目页面，点击 "Variables" 标签，添加以下环境变量：

```bash
# 数据库配置（使用PlanetScale的连接信息）
SPRING_DATASOURCE_URL=jdbc:mysql://your-host.psdb.cloud:3306/blog_db?useUnicode=true&characterEncoding=utf8&useSSL=true&requireSSL=true&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=your_username
SPRING_DATASOURCE_PASSWORD=your_password
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver

# JWT配置
JWT_SECRET=your-super-secret-key-change-this-in-production
JWT_EXPIRATION=86400000

# Spring Boot配置
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=prod
```

**重要提示：**
- 将 `your-host.psdb.cloud` 替换为PlanetScale的Host
- 将 `your_username` 和 `your_password` 替换为PlanetScale的用户名和密码
- `JWT_SECRET` 改为一个随机字符串（至少32位）

### 3.4 配置构建和启动命令

Railway会自动检测Spring Boot项目，但我们需要确保配置正确：

1. 在Railway项目页面，点击 "Settings"
2. 在 "Build Command" 中填写：
   ```bash
   cd backend && mvn clean package -DskipTests
   ```
3. 在 "Start Command" 中填写：
   ```bash
   cd backend && java -jar target/blog-backend-1.0.0.jar
   ```

### 3.5 部署

1. Railway会自动开始构建和部署
2. 等待部署完成（约3-5分钟）
3. 部署成功后，Railway会提供一个URL，例如：`https://your-app.railway.app`
4. **记录这个URL**，这是你的后端API地址，稍后配置前端时需要用到

### 3.6 测试后端

在浏览器访问：`https://your-app.railway.app/api/auth/user`

如果看到错误信息（这是正常的，因为需要认证），说明后端已经成功部署！

---

## 第四步：部署前端（Vercel）

### 4.1 修改前端配置

在部署前，我们需要修改前端代码以支持生产环境的API地址。

**我已经为你创建了配置文件，你只需要：**

1. 在 `frontend` 目录下创建 `.env.production` 文件（已创建）
2. 填入你的Railway后端地址

### 4.2 注册Vercel账号

1. 访问 [Vercel](https://vercel.com/)
2. 点击 "Sign Up"
3. 使用GitHub账号登录
4. 授权Vercel访问你的GitHub仓库

### 4.3 部署前端

1. 登录Vercel后，点击 "Add New..." → "Project"
2. 选择你的 `blog` 仓库
3. 配置项目：
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
4. 添加环境变量：
   - **Key**: `VITE_API_BASE_URL`
   - **Value**: `https://your-app.railway.app/api`（你的Railway后端地址）
5. 点击 "Deploy"

### 4.4 等待部署完成

- Vercel会自动构建和部署
- 部署完成后，会提供一个URL，例如：`https://blog-xxx.vercel.app`
- 这就是你的网站地址！

### 4.5 测试网站

1. 访问Vercel提供的URL
2. 测试注册和登录功能
3. 如果一切正常，恭喜你部署成功！🎉

---

## 第五步：配置自定义域名（可选）

### 5.1 购买域名

- 国内：阿里云、腾讯云（约10-50元/年）
- 国外：Namecheap、GoDaddy（约$10-15/年）

### 5.2 在Vercel配置域名

1. 在Vercel项目页面，点击 "Settings" → "Domains"
2. 输入你的域名，例如：`blog.yourdomain.com`
3. 按照提示配置DNS记录：
   - 类型：CNAME
   - 名称：blog（或@）
   - 值：cname.vercel-dns.com
4. 等待DNS生效（通常几分钟到几小时）

### 5.3 配置HTTPS

Vercel会自动为你的域名配置HTTPS证书，无需额外操作！

---

## 🔧 后续维护

### 更新代码

```bash
# 1. 修改代码
# 2. 提交到GitHub
git add .
git commit -m "Update code"
git push origin main

# 3. Railway和Vercel会自动重新部署
```

### 查看日志

- **Railway**: 在项目页面点击 "Deployments" → 选择部署 → 查看日志
- **Vercel**: 在项目页面点击 "Deployments" → 选择部署 → 查看日志

### 数据库管理

- 访问PlanetScale控制台
- 点击 "Console" 可以执行SQL查询
- 点击 "Branches" 可以创建数据库分支进行测试

---

## ❓ 常见问题

### Q1: Railway部署失败怎么办？

**检查：**
1. 环境变量是否正确配置
2. 数据库连接信息是否正确
3. 查看Railway的构建日志

**常见错误：**
- 数据库连接失败：检查PlanetScale的连接信息
- 端口错误：确保使用8080端口
- 内存不足：Railway免费版有512MB限制，如果不够可以升级

### Q2: 前端无法连接后端？

**检查：**
1. 前端环境变量 `VITE_API_BASE_URL` 是否正确
2. 后端是否正常运行（访问Railway提供的URL测试）
3. 浏览器控制台是否有CORS错误

**解决CORS问题：**
需要在后端添加CORS配置（我会帮你添加）

### Q3: 数据库连接失败？

**检查：**
1. PlanetScale的连接信息是否正确
2. 数据库是否已创建
3. 网络是否正常

**注意：** PlanetScale要求使用SSL连接，确保连接URL中包含 `useSSL=true`

### Q4: 如何重置数据库？

在PlanetScale控制台：
1. 点击 "Branches"
2. 创建新分支
3. 删除旧表，重新创建
4. 部署到生产环境

### Q5: 免费额度会用完吗？

- **Vercel**: 个人项目基本不会用完
- **Railway**: $5/月免费额度，小项目足够
- **PlanetScale**: 5GB存储，小项目足够

如果超出免费额度，会收到邮件通知。

---

## 📊 监控和统计

### Railway监控

- 在Railway项目页面可以查看：
  - CPU和内存使用情况
  - 请求数量
  - 日志

### Vercel分析

- Vercel提供基础的访问统计
- 可以集成Google Analytics获取更详细的数据

---

## 🎉 完成！

恭喜！你的博客网站现在已经完全免费地部署在云端了！

**你的网站地址：** `https://your-app.vercel.app`

**下一步可以做什么：**
- 添加更多功能（文章管理、评论等）
- 配置自定义域名
- 优化性能
- 添加监控和分析

---

## 📚 参考资源

- [Railway文档](https://docs.railway.app/)
- [Vercel文档](https://vercel.com/docs)
- [PlanetScale文档](https://docs.planetscale.com/)
- [Spring Boot部署指南](https://spring.io/guides/gs/spring-boot-for-azure/)

---

**需要帮助？** 查看日志或联系我！

