# 部署后端到Railway - 详细步骤

## ✅ 当前进度

- ✅ 数据库已创建
- ✅ users表已创建
- ⏭️ **下一步：部署后端**

---

## 📋 步骤3：部署后端到Railway

### 步骤1：在Railway中创建后端服务

1. **在Railway项目页面**（你当前所在的页面）
2. **点击左侧的 "+ New" 按钮**（在MySQL服务上方或下方）
3. **选择 "GitHub Repo"** 或 **"Deploy from GitHub repo"**
4. **选择你的仓库**：
   - 选择 `wyx1223221/blog` 仓库
   - **重要**：在 "Root Directory" 中选择 `backend` 文件夹
   - 点击 "Deploy"

### 步骤2：等待初始部署

- Railway会自动开始构建和部署
- 等待2-3分钟
- 初始部署可能会失败（因为还没有配置数据库连接），这是正常的

### 步骤3：配置环境变量

1. **点击创建的后端服务**（左侧会显示一个新的服务卡片）
2. **切换到 "Variables" 标签**
3. **添加以下环境变量**（点击 "New Variable" 逐个添加）：

```
SPRING_DATASOURCE_URL=jdbc:mysql://mysql.railway.internal:3306/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTILYPDOI
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
JWT_SECRET=my-super-secret-jwt-key-2024-change-this-to-random-string-at-least-32-chars
JWT_EXPIRATION=86400000
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=prod
```

**注意**：
- `SPRING_DATASOURCE_PASSWORD` 使用你从MySQL Variables中复制的 `MYSQLPASSWORD` 值
- `JWT_SECRET` 改为一个至少32位的随机字符串（例如：`my-blog-jwt-secret-key-2024-very-long-random-string`）

### 步骤4：重新部署

1. **配置完环境变量后**，Railway会自动重新部署
2. **或者点击 "Redeploy" 按钮**手动触发
3. **等待部署完成**（约3-5分钟）

### 步骤5：获取后端URL

1. **部署成功后**，在服务卡片上会显示一个URL
2. **或者点击服务 → "Settings" 标签 → "Generate Domain"**
3. **记录这个URL**，例如：`https://your-app.railway.app`
4. **这是你的后端API地址！**

### 步骤6：测试后端

在浏览器访问：
```
https://your-app.railway.app/api/auth/user
```

如果看到错误信息（这是正常的，因为需要认证），说明后端部署成功！

---

## 🎯 快速操作指南

### 1. 创建后端服务
- 点击 "+ New" → "GitHub Repo"
- 选择 `wyx1223221/blog`
- Root Directory: `backend`
- 点击 "Deploy"

### 2. 配置环境变量
- 点击后端服务 → "Variables" 标签
- 添加上面列出的7个环境变量
- 使用你之前保存的数据库连接信息

### 3. 等待部署完成
- 查看 "Deployments" 标签
- 等待部署成功
- 记录后端URL

---

## ⚠️ 常见问题

### Q: 找不到Root Directory选项？
A: 在连接GitHub仓库后，会有一个设置页面，在那里可以设置Root Directory。

### Q: 部署失败怎么办？
A: 
1. 检查 "Deployments" 标签中的日志
2. 确保环境变量都配置正确
3. 确保数据库连接信息正确

### Q: 如何查看部署日志？
A: 点击后端服务 → "Deployments" 标签 → 选择最新的部署 → 查看日志

---

## 📝 下一步

后端部署成功后，下一步是：
1. 部署前端到Vercel
2. 配置前端连接后端API
3. 测试完整功能

按照 `QUICK_START.md` 的步骤4继续操作！

