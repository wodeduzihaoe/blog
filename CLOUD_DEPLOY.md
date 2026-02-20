# 博客网站云部署完整指南

本指南将帮助你将博客网站部署到各种云平台。

## 📋 目录

1. [部署方案对比](#部署方案对比)
2. [国内云平台部署](#国内云平台部署)
3. [国外云平台部署](#国外云平台部署)
4. [混合部署方案](#混合部署方案)
5. [数据库云服务](#数据库云服务)
6. [域名和HTTPS配置](#域名和https配置)
7. [监控和维护](#监控和维护)

---

## 🎯 部署方案对比

| 方案 | 优点 | 缺点 | 适用场景 | 成本 |
|------|------|------|----------|------|
| **ECS + 自建服务** | 完全控制，灵活 | 需要自己维护 | 学习/小项目 | 低-中 |
| **容器服务** | 易扩展，易管理 | 需要学习Docker | 中小型项目 | 中 |
| **Serverless** | 按需付费，自动扩展 | 冷启动，限制多 | 小型项目 | 低 |
| **混合部署** | 成本优化 | 配置复杂 | 中大型项目 | 中-高 |

---

## 🇨🇳 国内云平台部署

### 方案一：阿里云 ECS + RDS（推荐新手）

#### 1. 购买服务

**ECS 服务器：**
- 进入 [阿里云ECS控制台](https://ecs.console.aliyun.com/)
- 选择：1核2G，1M带宽（学生优惠约10元/月）
- 系统：Ubuntu 20.04 或 CentOS 7
- 地区：选择离你最近的

**RDS 数据库（可选，也可在ECS上自建）：**
- 进入 [RDS控制台](https://rds.console.aliyun.com/)
- 选择：MySQL 8.0，基础版
- 配置：1核1G（约30元/月）

#### 2. 配置安全组

在ECS控制台配置安全组规则：
- 开放端口：22（SSH）、80（HTTP）、443（HTTPS）、8080（后端API）
- 协议：TCP
- 授权对象：0.0.0.0/0

#### 3. 连接服务器

```bash
# Windows 使用 PowerShell 或 PuTTY
ssh root@your-server-ip

# 或使用阿里云控制台的远程连接
```

#### 4. 安装基础环境

```bash
# Ubuntu/Debian
apt update && apt upgrade -y
apt install -y openjdk-8-jdk maven mysql-server nginx git

# CentOS
yum update -y
yum install -y java-1.8.0-openjdk maven mysql-server nginx git
systemctl start mysqld
systemctl enable mysqld
```

#### 5. 配置 MySQL

**如果使用ECS自建MySQL：**
```bash
# 登录MySQL（首次需要查看临时密码）
sudo cat /var/log/mysqld.log | grep password
mysql -u root -p

# 创建数据库和用户
CREATE DATABASE blog_db DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'bloguser'@'%' IDENTIFIED BY 'your_strong_password';
GRANT ALL PRIVILEGES ON blog_db.* TO 'bloguser'@'%';
FLUSH PRIVILEGES;
EXIT;

# 导入数据库结构
mysql -u bloguser -p blog_db < /opt/blog/backend/src/main/resources/db/schema.sql
```

**如果使用RDS：**
- 在RDS控制台创建数据库和用户
- 记录连接地址、端口、用户名、密码

#### 6. 部署后端

```bash
# 上传项目（方式一：Git）
cd /opt
git clone your-repo-url blog
cd blog

# 或使用SCP上传（方式二）
# 在本地执行：scp -r blog root@your-server-ip:/opt/

# 配置后端
cd /opt/blog/backend
nano src/main/resources/application.yml

# 修改数据库配置：
# url: jdbc:mysql://your-rds-endpoint:3306/blog_db?...
# username: bloguser
# password: your_password

# 打包
mvn clean package -DskipTests

# 创建启动脚本
cat > /opt/blog/backend/start.sh << 'EOF'
#!/bin/bash
cd /opt/blog/backend
nohup java -jar target/blog-backend-1.0.0.jar > app.log 2>&1 &
EOF
chmod +x /opt/blog/backend/start.sh

# 创建systemd服务（推荐）
cat > /etc/systemd/system/blog-backend.service << EOF
[Unit]
Description=Blog Backend Service
After=network.target mysql.service

[Service]
Type=simple
User=root
WorkingDirectory=/opt/blog/backend
ExecStart=/usr/bin/java -jar /opt/blog/backend/target/blog-backend-1.0.0.jar
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

# 启动服务
systemctl daemon-reload
systemctl enable blog-backend
systemctl start blog-backend
systemctl status blog-backend

# 查看日志
journalctl -u blog-backend -f
```

#### 7. 部署前端

```bash
cd /opt/blog/frontend

# 安装依赖
npm install

# 构建
npm run build

# 配置Nginx
cat > /etc/nginx/sites-available/blog << EOF
server {
    listen 80;
    server_name your-domain.com your-server-ip;

    # 前端静态文件
    location / {
        root /opt/blog/frontend/dist;
        try_files \$uri \$uri/ /index.html;
        index index.html;
    }

    # 后端API代理
    location /api {
        proxy_pass http://localhost:8080;
        proxy_set_header Host \$host;
        proxy_set_header X-Real-IP \$remote_addr;
        proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto \$scheme;
        
        # 解决跨域
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
        add_header Access-Control-Allow-Headers 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    }
}
EOF

# 启用配置
ln -s /etc/nginx/sites-available/blog /etc/nginx/sites-enabled/
rm -f /etc/nginx/sites-enabled/default
nginx -t
systemctl restart nginx
systemctl enable nginx
```

#### 8. 配置防火墙

```bash
# Ubuntu/Debian
ufw allow 22
ufw allow 80
ufw allow 443
ufw allow 8080
ufw enable

# CentOS
firewall-cmd --permanent --add-port=22/tcp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

---

### 方案二：阿里云容器服务 ACK（推荐进阶）

#### 1. 创建容器镜像仓库

```bash
# 在本地构建镜像
cd backend
mvn clean package -DskipTests
docker build -t registry.cn-hangzhou.aliyuncs.com/your-namespace/blog-backend:latest .

cd ../frontend
docker build -t registry.cn-hangzhou.aliyuncs.com/your-namespace/blog-frontend:latest .

# 登录阿里云容器镜像服务
docker login --username=your-username registry.cn-hangzhou.aliyuncs.com

# 推送镜像
docker push registry.cn-hangzhou.aliyuncs.com/your-namespace/blog-backend:latest
docker push registry.cn-hangzhou.aliyuncs.com/your-namespace/blog-frontend:latest
```

#### 2. 创建ACK集群

- 进入 [容器服务控制台](https://cs.console.aliyun.com/)
- 创建Kubernetes集群（或使用Serverless Kubernetes，更简单）
- 配置节点：1-2个节点即可

#### 3. 部署应用

使用Kubernetes YAML或通过控制台部署：

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blog-backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: blog-backend
  template:
    metadata:
      labels:
        app: blog-backend
    spec:
      containers:
      - name: backend
        image: registry.cn-hangzhou.aliyuncs.com/your-namespace/blog-backend:latest
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_DATASOURCE_URL
          value: "jdbc:mysql://your-rds-endpoint:3306/blog_db"
        - name: SPRING_DATASOURCE_USERNAME
          value: "bloguser"
        - name: SPRING_DATASOURCE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
---
apiVersion: v1
kind: Service
metadata:
  name: blog-backend-service
spec:
  selector:
    app: blog-backend
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

---

### 方案三：腾讯云部署

步骤与阿里云类似：

1. **购买CVM服务器**（[腾讯云CVM](https://console.cloud.tencent.com/cvm)）
2. **购买云数据库MySQL**（[CDB控制台](https://console.cloud.tencent.com/cdb)）
3. **配置安全组**（开放80、443、8080端口）
4. **按照阿里云步骤部署**

---

## 🌍 国外云平台部署

### 方案一：Vercel（前端）+ Railway/Render（后端）

#### 前端部署到Vercel（免费）

```bash
# 1. 安装Vercel CLI
npm i -g vercel

# 2. 在frontend目录下
cd frontend

# 3. 修改vite.config.js，添加生产环境API地址
# base: '/',
# build: {
#   outDir: 'dist'
# }

# 4. 创建vercel.json
cat > vercel.json << EOF
{
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "dist"
      }
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/$1"
    }
  ]
}
EOF

# 5. 修改package.json，添加build脚本
# "scripts": {
#   "build": "vite build"
# }

# 6. 部署
vercel

# 或通过GitHub自动部署
# 在Vercel网站连接GitHub仓库，自动部署
```

#### 后端部署到Railway（推荐，免费额度）

1. 访问 [Railway](https://railway.app/)
2. 使用GitHub登录
3. 点击"New Project" -> "Deploy from GitHub repo"
4. 选择你的仓库，选择backend目录
5. 配置环境变量：
   - `SPRING_DATASOURCE_URL`
   - `SPRING_DATASOURCE_USERNAME`
   - `SPRING_DATASOURCE_PASSWORD`
6. Railway会自动构建和部署

#### 后端部署到Render（免费，但有限制）

1. 访问 [Render](https://render.com/)
2. 连接GitHub仓库
3. 创建"Web Service"
4. 配置：
   - Build Command: `cd backend && mvn clean package -DskipTests`
   - Start Command: `cd backend && java -jar target/blog-backend-1.0.0.jar`
   - Environment: Java

---

### 方案二：AWS部署

#### 使用AWS Elastic Beanstalk（最简单）

1. 安装AWS CLI和EB CLI
```bash
pip install awsebcli
```

2. 初始化EB
```bash
cd backend
eb init -p java-8 blog-backend
eb create blog-backend-env
eb deploy
```

3. 配置环境变量
```bash
eb setenv SPRING_DATASOURCE_URL=jdbc:mysql://...
```

#### 使用AWS EC2（类似阿里云ECS）

步骤与阿里云ECS完全相同。

---

### 方案三：Heroku部署

#### 后端部署

```bash
# 1. 安装Heroku CLI
# 下载：https://devcenter.heroku.com/articles/heroku-cli

# 2. 登录
heroku login

# 3. 创建应用
cd backend
heroku create your-app-name

# 4. 添加MySQL插件
heroku addons:create cleardb:ignite

# 5. 获取数据库URL
heroku config:get CLEARDB_DATABASE_URL

# 6. 配置环境变量
heroku config:set SPRING_DATASOURCE_URL=jdbc:mysql://...

# 7. 部署
git push heroku main

# 8. 查看日志
heroku logs --tail
```

#### 前端部署

Heroku也可以部署前端，但更推荐使用Vercel或Netlify。

---

## 🔄 混合部署方案

### 前端：Vercel/Netlify（免费CDN）
### 后端：Railway/Render（免费托管）
### 数据库：PlanetScale/Supabase（免费MySQL）

这是**最经济的方案**，完全免费（在免费额度内）。

---

## 🗄️ 数据库云服务

### 国内

1. **阿里云RDS MySQL**
   - 价格：约30元/月起
   - 优点：稳定，备份自动
   - 适合：生产环境

2. **腾讯云CDB MySQL**
   - 价格：约30元/月起
   - 类似阿里云RDS

### 国外

1. **PlanetScale**（推荐，免费）
   - 免费额度：5GB存储
   - 基于Vitess，性能好
   - 网址：https://planetscale.com/

2. **Supabase**
   - 免费PostgreSQL（也可用MySQL）
   - 网址：https://supabase.com/

3. **Railway Database**
   - 在Railway上直接创建MySQL数据库
   - 免费额度：5美元/月

---

## 🌐 域名和HTTPS配置

### 1. 购买域名

- 国内：阿里云、腾讯云
- 国外：Namecheap、GoDaddy

### 2. 解析域名

在域名管理中添加A记录：
- 类型：A
- 主机记录：@（或www）
- 记录值：你的服务器IP
- TTL：600

### 3. 配置HTTPS（使用Let's Encrypt免费证书）

```bash
# 安装Certbot
apt install certbot python3-certbot-nginx -y

# 获取证书
certbot --nginx -d your-domain.com -d www.your-domain.com

# 自动续期（证书3个月过期，会自动续期）
certbot renew --dry-run

# 设置自动续期定时任务
crontab -e
# 添加：0 0 1 * * certbot renew --quiet
```

### 4. 修改Nginx配置（自动完成）

Certbot会自动修改Nginx配置，添加HTTPS支持。

---

## 📊 监控和维护

### 1. 日志查看

```bash
# 后端日志
journalctl -u blog-backend -f
# 或
tail -f /opt/blog/backend/app.log

# Nginx日志
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### 2. 性能监控

**使用阿里云监控：**
- 在ECS控制台启用云监控
- 查看CPU、内存、网络使用情况

**使用开源工具：**
```bash
# 安装htop
apt install htop
htop

# 查看端口占用
netstat -tlnp | grep 8080
```

### 3. 自动备份

```bash
# 创建数据库备份脚本
cat > /opt/blog/backup.sh << 'EOF'
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/opt/blog/backups"
mkdir -p $BACKUP_DIR

# 备份数据库
mysqldump -u bloguser -p'your_password' blog_db > $BACKUP_DIR/db_$DATE.sql

# 删除7天前的备份
find $BACKUP_DIR -name "db_*.sql" -mtime +7 -delete
EOF

chmod +x /opt/blog/backup.sh

# 设置定时任务（每天凌晨2点备份）
crontab -e
# 添加：0 2 * * * /opt/blog/backup.sh
```

### 4. 更新代码

```bash
# 如果使用Git
cd /opt/blog
git pull origin main

# 重新构建后端
cd backend
mvn clean package -DskipTests
systemctl restart blog-backend

# 重新构建前端
cd ../frontend
npm run build
systemctl reload nginx
```

---

## 🚀 快速开始（推荐方案）

**对于新手，推荐这个最简单经济的方案：**

1. **购买阿里云ECS**（学生优惠约10元/月）
   - 1核2G，1M带宽
   - Ubuntu 20.04

2. **在ECS上自建MySQL**（免费）
   - 按照上面的步骤安装和配置

3. **部署应用**
   - 按照"方案一：阿里云ECS"的步骤部署

4. **配置域名和HTTPS**（可选）
   - 购买域名（约10元/年）
   - 使用Let's Encrypt免费证书

**总成本：约10-20元/月**

---

## ❓ 常见问题

### Q: 前端无法访问后端API？
A: 检查：
1. 后端服务是否启动：`systemctl status blog-backend`
2. Nginx代理配置是否正确
3. 防火墙是否开放8080端口
4. 前端API地址配置是否正确

### Q: 数据库连接失败？
A: 检查：
1. MySQL服务是否启动：`systemctl status mysql`
2. 数据库用户权限是否正确
3. 安全组是否开放3306端口（如果使用RDS）
4. 连接地址是否正确

### Q: 如何查看错误日志？
A: 
```bash
# 后端日志
journalctl -u blog-backend -n 100

# Nginx错误日志
tail -f /var/log/nginx/error.log
```

### Q: 如何重启服务？
A:
```bash
# 重启后端
systemctl restart blog-backend

# 重启Nginx
systemctl restart nginx

# 重启MySQL
systemctl restart mysql
```

---

## 📚 参考资源

- [阿里云ECS文档](https://help.aliyun.com/product/25365.html)
- [Nginx配置指南](https://nginx.org/en/docs/)
- [Let's Encrypt文档](https://letsencrypt.org/docs/)
- [Docker官方文档](https://docs.docker.com/)

---

**祝你部署顺利！如有问题，请查看日志或联系我。** 🎉

