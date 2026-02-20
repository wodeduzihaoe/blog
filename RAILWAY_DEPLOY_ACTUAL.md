# Railway 实际部署操作步骤

## ✅ 当前状态

从你的界面可以看到：
- ✅ 启动命令已配置：`cd backend && java -jar target/blog-backend-1.0.0.jar`
- ✅ 在Settings页面
- ⚠️ 服务显示 "Service is offline"

---

## 🚀 启动部署的正确方法

### 方法一：通过顶部导航栏（最直接）

1. **查看页面顶部**，在 "Settings" 标签旁边
2. **查找 "Deploy" 按钮**（可能在右上角，紫色按钮）
3. **或查找带火箭图标的按钮**
4. **点击触发部署**

### 方法二：切换到Deployments页面

1. **点击 "Deployments" 标签**（在Settings旁边）
2. **在Deployments页面**，应该会看到：
   - "Make a deployment to get started →" 链接
   - 或 "Deploy" 按钮
   - 或 "Redeploy" 按钮
3. **点击触发部署**

### 方法三：通过代码推送（自动触发）

如果Railway已经连接到GitHub仓库，推送代码会自动触发部署：

```bash
# 在项目目录执行
cd C:\Users\yixian\Desktop\blog
git add .
git commit -m "Trigger deployment"
git push new-origin main
```

Railway会自动检测代码变更并开始部署。

### 方法四：检查Source设置

1. **在Settings页面，点击左侧导航栏的 "Source"**
2. **确保GitHub仓库已连接**
3. **如果有 "Deploy" 或 "Redeploy" 按钮，点击它**

---

## 📋 需要确认的配置

在部署前，确保以下配置正确：

### 1. Root Directory（重要！）

1. **点击左侧导航栏的 "Source"**
2. **查找 "Root Directory" 设置**
3. **确保设置为：`backend`**
4. **如果没有设置，输入 `backend` 并保存**

### 2. 环境变量

1. **切换到 "Variables" 标签**
2. **确保所有7个环境变量都已添加**：
   - SPRING_DATASOURCE_URL
   - SPRING_DATASOURCE_USERNAME
   - SPRING_DATASOURCE_PASSWORD
   - SPRING_DATASOURCE_DRIVER_CLASS_NAME
   - JWT_SECRET
   - JWT_EXPIRATION
   - SERVER_PORT
   - SPRING_PROFILES_ACTIVE

### 3. 启动命令（已配置）

- ✅ 已配置：`cd backend && java -jar target/blog-backend-1.0.0.jar`

---

## 🔍 如果找不到Deploy按钮

### 检查点：

1. **查看页面顶部**：
   - 查找紫色按钮
   - 查找带火箭图标的按钮
   - 查找 "Deploy" 文字

2. **查看Deployments页面**：
   - 切换到 "Deployments" 标签
   - 应该会有部署相关的按钮或链接

3. **查看Architecture页面**：
   - 切换到 "Architecture" 标签
   - 在服务卡片上可能有操作按钮

4. **使用代码推送**：
   - 如果找不到按钮，直接推送代码
   - Railway会自动检测并部署

---

## 🎯 推荐操作流程

### 步骤1：检查Root Directory

1. Settings → 点击左侧 "Source"
2. 确保 Root Directory = `backend`

### 步骤2：检查环境变量

1. 切换到 "Variables" 标签
2. 确认所有变量都已添加

### 步骤3：触发部署

**选项A**：推送代码（推荐）
```bash
git add .
git commit -m "Deploy"
git push new-origin main
```

**选项B**：在界面中查找Deploy按钮
- 顶部导航栏
- Deployments页面
- Architecture页面

### 步骤4：查看部署状态

1. 切换到 "Deployments" 标签
2. 查看部署进度
3. 等待部署完成

---

## 📝 告诉我你看到了什么

如果还是找不到Deploy按钮，请告诉我：

1. **在Settings页面顶部看到了什么按钮？**
2. **切换到Deployments页面后看到了什么？**
3. **左侧导航栏有哪些选项？**

这样我可以给你更准确的指导！

