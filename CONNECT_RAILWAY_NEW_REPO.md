# 在Railway中连接新GitHub仓库

## ✅ 已完成

- ✅ 代码已推送到新仓库：https://github.com/wodeduzihaoe/blog

---

## 📋 下一步：在Railway中连接新仓库

### 步骤1：在Railway中创建后端服务

1. **在Railway项目页面**（已打开）
2. **点击左侧的 "+ New" 按钮**（在MySQL服务上方或下方）
3. **选择 "GitHub Repo"** 或 **"Deploy from GitHub repo"**

### 步骤2：授权新GitHub账号

1. **点击 "Connect GitHub" 或类似按钮**
2. **使用新GitHub账号登录**（wodeduzihaoe）
   - 如果提示授权，点击 "Authorize Railway"
   - 新账号应该不会被标记，可以正常授权

### 步骤3：选择仓库

1. **在仓库列表中选择** `wodeduzihaoe/blog`
2. **重要配置**：
   - **Root Directory**: 选择 `backend` 文件夹
   - 其他设置保持默认
3. **点击 "Deploy" 或 "Add Service"**

### 步骤4：等待初始部署

- Railway会自动开始构建和部署
- 等待2-3分钟
- 初始部署可能会失败（因为还没有配置数据库连接），这是正常的

### 步骤5：配置环境变量

1. **点击创建的后端服务卡片**（左侧会显示新服务）
2. **切换到 "Variables" 标签**
3. **添加以下环境变量**（点击 "New Variable" 逐个添加）：

```
SPRING_DATASOURCE_URL=jdbc:mysql://mysql.railway.internal:3306/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTILYPDOI
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
JWT_SECRET=my-blog-jwt-secret-key-2024-very-long-random-string-at-least-32-chars
JWT_EXPIRATION=86400000
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=prod
```

**注意**：
- `SPRING_DATASOURCE_PASSWORD` 使用你从MySQL Variables中复制的 `MYSQLPASSWORD` 值
- `JWT_SECRET` 改为一个至少32位的随机字符串

### 步骤6：重新部署

1. **配置完环境变量后**，Railway会自动重新部署
2. **等待3-5分钟**
3. **在 "Deployments" 标签中查看部署进度**

### 步骤7：获取后端URL

部署成功后：
1. 在服务卡片上会显示一个URL
2. 或者点击服务 → "Settings" 标签 → "Generate Domain"
3. **记录这个URL**，例如：`https://your-app.railway.app`
4. **这是你的后端API地址！**

### 步骤8：测试后端

在浏览器访问：
```
https://your-app.railway.app/api/auth/user
```

如果看到错误信息（这是正常的，因为需要认证），说明后端部署成功！

---

## 🎯 快速操作

1. 点击 "+ New" → "GitHub Repo"
2. 使用新GitHub账号（wodeduzihaoe）授权
3. 选择 `wodeduzihaoe/blog` 仓库
4. Root Directory: `backend`
5. 配置7个环境变量
6. 等待部署完成
7. 记录后端URL

---

## ⚠️ 如果授权还是失败

如果新账号也无法授权（虽然可能性很小），可以使用Railway CLI部署：

```bash
# 安装Railway CLI
npm install -g @railway/cli

# 登录
railway login

# 在backend目录中
cd backend
railway init
railway up
```

---

## 📝 下一步

后端部署成功后，下一步是：
1. 部署前端到Vercel
2. 配置前端连接后端API
3. 测试完整功能

按照 `QUICK_START.md` 的步骤4继续操作！

