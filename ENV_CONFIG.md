# 环境变量配置说明

## 前端环境变量（Vercel）

在Vercel项目设置中添加以下环境变量：

```
VITE_API_BASE_URL=https://your-railway-app.railway.app/api
```

**如何设置：**
1. 登录Vercel
2. 进入你的项目
3. 点击 "Settings" → "Environment Variables"
4. 添加变量：
   - Key: `VITE_API_BASE_URL`
   - Value: `https://your-railway-app.railway.app/api`（替换为你的Railway后端地址）
   - Environment: Production, Preview, Development（全选）

## 后端环境变量（Railway）

在Railway项目设置中添加以下环境变量：

### 数据库配置（PlanetScale）

```
SPRING_DATASOURCE_URL=jdbc:mysql://your-host.psdb.cloud:3306/blog_db?useUnicode=true&characterEncoding=utf8&useSSL=true&requireSSL=true&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=your_username
SPRING_DATASOURCE_PASSWORD=your_password
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
```

### JWT配置

```
JWT_SECRET=your-super-secret-key-change-this-to-random-string-at-least-32-chars
JWT_EXPIRATION=86400000
```

### Spring Boot配置

```
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=prod
```

**如何设置：**
1. 登录Railway
2. 进入你的项目
3. 点击 "Variables" 标签
4. 点击 "New Variable" 添加每个变量

## 本地开发环境变量

### 前端（可选）

创建 `frontend/.env.local` 文件：

```
VITE_API_BASE_URL=http://localhost:8080/api
```

### 后端

修改 `backend/src/main/resources/application.yml` 中的数据库配置即可。

