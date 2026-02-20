# Railway 启动部署和查看状态

## 🚀 启动部署

### 方法一：通过Settings页面触发（推荐）

1. **切换到 "Settings" 标签**
2. **查找右上角的 "Deploy" 按钮**（紫色按钮，带火箭图标）
3. **点击 "Deploy" 按钮**
4. **或按快捷键 `Enter`**

### 方法二：通过代码推送触发

1. **如果你已经配置了GitHub连接**
2. **推送代码到GitHub仓库**：
   ```bash
   git add .
   git commit -m "Update code"
   git push new-origin main
   ```
3. **Railway会自动检测代码变更并开始部署**

### 方法三：手动触发

1. **在Deployments页面**
2. **点击 "Make a deployment to get started →" 链接**
3. **或查找 "Redeploy" 或 "Deploy" 按钮**

---

## 📊 查看部署状态

### 步骤1：切换到Deployments标签

1. **在服务详情页面**（你当前所在的页面）
2. **点击 "Deployments" 标签**（已经在Deployments页面了）

### 步骤2：查看部署进度

部署开始后，你会看到：

1. **部署卡片**
   - 显示部署状态（Building、Deploying、Active等）
   - 显示部署时间
   - 显示部署ID

2. **状态指示器**
   - 🟡 **Building** - 正在构建
   - 🔵 **Deploying** - 正在部署
   - 🟢 **Active** - 部署成功，服务运行中
   - 🔴 **Failed** - 部署失败

3. **部署日志**
   - 点击部署卡片可以查看详细日志
   - 查看构建和部署过程

### 步骤3：查看实时日志

1. **点击部署卡片**
2. **查看 "Logs" 部分**
3. **实时查看部署日志**

---

## 🔍 常见部署状态

### Building（构建中）
- Railway正在构建你的应用
- 执行 `mvn clean package` 等构建命令
- 通常需要2-3分钟

### Deploying（部署中）
- 构建完成，正在启动服务
- 启动Spring Boot应用
- 通常需要1-2分钟

### Active（运行中）
- ✅ 部署成功！
- 服务正在运行
- 可以访问后端API

### Failed（失败）
- ❌ 部署失败
- 查看日志找出问题
- 常见原因：
  - 环境变量配置错误
  - 构建失败
  - 数据库连接失败

---

## ⚠️ 如果部署失败

### 检查清单：

1. **检查环境变量**
   - 确保所有7个变量都已添加
   - 确保数据库密码正确
   - 确保Root Directory设置为 `backend`

2. **查看部署日志**
   - 点击失败的部署
   - 查看错误信息
   - 常见错误：
     - `Connection refused` - 数据库连接失败
     - `Build failed` - 构建失败
     - `Port already in use` - 端口冲突

3. **检查Root Directory**
   - Settings → Source → Root Directory = `backend`

4. **检查启动命令**
   - Settings → Deploy → Custom Start Command
   - 应该是：`cd backend && java -jar target/blog-backend-1.0.0.jar`

---

## 📝 部署成功后的操作

### 1. 获取后端URL

部署成功后：
1. **在服务卡片上会显示URL**
2. **或点击服务 → Settings → Networking → Generate Domain**
3. **记录这个URL**，例如：`https://blog-production-xxxx.up.railway.app`

### 2. 测试后端

在浏览器访问：
```
https://your-app.railway.app/api/auth/user
```

如果看到错误信息（这是正常的，因为需要认证），说明后端部署成功！

### 3. 查看服务状态

- **Architecture页面**：查看服务是否在线
- **Metrics页面**：查看CPU、内存使用情况
- **Logs页面**：查看实时日志

---

## 🎯 快速操作

1. **启动部署**：Settings → 点击 "Deploy" 按钮
2. **查看状态**：Deployments标签 → 查看部署卡片
3. **查看日志**：点击部署卡片 → 查看Logs
4. **获取URL**：部署成功后 → Settings → Networking → Generate Domain

