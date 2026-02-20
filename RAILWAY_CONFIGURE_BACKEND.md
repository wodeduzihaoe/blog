# 配置Railway后端服务

## ✅ 当前状态

- ✅ 后端服务已创建（blog服务）
- ⏭️ 需要配置：Root Directory、环境变量、启动命令

---

## 📋 配置步骤

### 步骤1：配置Root Directory（重要！）

1. **在Settings页面，查找 "Source" 部分**
   - 可能在左侧导航栏中点击 "Source"
   - 或滚动页面查找 "Source" 设置

2. **找到 "Root Directory" 设置**
   - 输入：`backend`
   - 或选择 `backend` 文件夹

3. **保存更改**（如果有"Apply"或"Save"按钮）

### 步骤2：配置启动命令

1. **在Settings页面的 "Deploy" 部分**
2. **找到 "Custom Start Command"**
3. **点击 "+ Start Command"**
4. **输入启动命令**：
   ```
   cd backend && java -jar target/blog-backend-1.0.0.jar
   ```

### 步骤3：配置环境变量（最重要！）

1. **切换到 "Variables" 标签**（在服务导航栏中）
2. **点击 "+ New Variable" 或 "Add Variable"**
3. **逐个添加以下环境变量**：

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

### 步骤4：触发部署

1. **配置完成后，点击 "Deploy" 按钮**（右上角的紫色按钮）
2. **或Railway会自动检测更改并开始部署**
3. **等待3-5分钟**

### 步骤5：查看部署状态

1. **切换到 "Deployments" 标签**
2. **查看部署日志**
3. **等待部署完成**

### 步骤6：获取后端URL

部署成功后：
1. **在服务卡片上会显示URL**
2. **或点击服务 → "Settings" → "Networking" → "Generate Domain"**
3. **记录这个URL**，例如：`https://blog-production-xxxx.up.railway.app`

---

## 🎯 快速操作清单

- [ ] 配置Root Directory为 `backend`
- [ ] 配置启动命令：`cd backend && java -jar target/blog-backend-1.0.0.jar`
- [ ] 切换到Variables标签
- [ ] 添加7个环境变量
- [ ] 点击Deploy
- [ ] 等待部署完成
- [ ] 记录后端URL

---

## ⚠️ 常见问题

### Q: 找不到Root Directory设置？
A: 在Settings页面查找 "Source" 部分，或点击左侧导航栏的 "Source"

### Q: 部署失败怎么办？
A: 
1. 检查 "Deployments" 标签中的日志
2. 确保Root Directory设置为 `backend`
3. 确保环境变量都配置正确
4. 确保启动命令正确

### Q: 如何查看部署日志？
A: 点击 "Deployments" 标签 → 选择最新的部署 → 查看日志

---

## 📝 下一步

后端部署成功后，下一步是：
1. 部署前端到Vercel
2. 配置前端连接后端API
3. 测试完整功能

