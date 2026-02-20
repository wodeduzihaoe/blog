# 🚀 免费部署检查清单

按照以下步骤逐一完成部署，每完成一步就打勾 ✅

## 准备工作

- [ ] 安装Git（如果还没有）
- [ ] 注册GitHub账号
- [ ] 将代码推送到GitHub仓库

## 第一步：部署数据库（PlanetScale）

- [ ] 注册PlanetScale账号（使用GitHub登录）
- [ ] 创建数据库 `blog_db`
- [ ] 获取数据库连接信息（Host、Username、Password）
- [ ] 在PlanetScale Console中执行SQL创建users表
- [ ] 部署到生产环境

**预计时间：10分钟**

## 第二步：部署后端（Railway）

- [ ] 注册Railway账号（使用GitHub登录）
- [ ] 创建新项目，连接GitHub仓库
- [ ] 设置Root Directory为 `backend`
- [ ] 添加环境变量：
  - [ ] `SPRING_DATASOURCE_URL`
  - [ ] `SPRING_DATASOURCE_USERNAME`
  - [ ] `SPRING_DATASOURCE_PASSWORD`
  - [ ] `SPRING_DATASOURCE_DRIVER_CLASS_NAME`
  - [ ] `JWT_SECRET`
  - [ ] `JWT_EXPIRATION`
  - [ ] `SERVER_PORT`
  - [ ] `SPRING_PROFILES_ACTIVE`
- [ ] 等待部署完成
- [ ] 记录Railway提供的后端URL（例如：`https://xxx.railway.app`）
- [ ] 测试后端API（访问 `https://xxx.railway.app/api/auth/user`）

**预计时间：15分钟**

## 第三步：部署前端（Vercel）

- [ ] 注册Vercel账号（使用GitHub登录）
- [ ] 创建新项目，连接GitHub仓库
- [ ] 设置Root Directory为 `frontend`
- [ ] 配置Framework Preset为 `Vite`
- [ ] 添加环境变量：
  - [ ] `VITE_API_BASE_URL` = `https://xxx.railway.app/api`（你的Railway后端地址）
- [ ] 点击Deploy
- [ ] 等待部署完成
- [ ] 记录Vercel提供的前端URL（例如：`https://xxx.vercel.app`）

**预计时间：10分钟**

## 第四步：测试网站

- [ ] 访问Vercel提供的前端URL
- [ ] 测试注册功能
- [ ] 测试登录功能
- [ ] 检查浏览器控制台是否有错误
- [ ] 检查网络请求是否正常

**预计时间：5分钟**

## 完成！

- [ ] 所有功能正常
- [ ] 网站可以正常访问
- [ ] 用户注册和登录功能正常

**总预计时间：40分钟**

---

## 如果遇到问题

1. **后端部署失败**
   - 检查Railway日志
   - 检查环境变量是否正确
   - 检查数据库连接信息

2. **前端无法连接后端**
   - 检查Vercel环境变量 `VITE_API_BASE_URL` 是否正确
   - 检查后端是否正常运行
   - 检查浏览器控制台的错误信息

3. **数据库连接失败**
   - 检查PlanetScale连接信息
   - 确保连接URL包含 `useSSL=true`
   - 检查数据库是否已创建

---

## 下一步

- [ ] 配置自定义域名（可选）
- [ ] 添加更多功能
- [ ] 优化性能
- [ ] 添加监控

