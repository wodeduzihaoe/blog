# 免费数据库替代方案

由于PlanetScale需要GitHub授权且可能在中国无法访问，这里提供几个替代方案。

## 🎯 推荐方案：Railway MySQL（最佳选择）

### 优点
- ✅ **完全免费**：每月$5信用额度，足够小项目使用
- ✅ **邮箱注册**：不需要GitHub授权
- ✅ **MySQL支持**：原生MySQL，无需修改代码
- ✅ **访问友好**：对中国用户友好，速度快
- ✅ **简单易用**：一键创建数据库
- ✅ **自动备份**：自动数据备份

### 注册步骤
1. 访问：https://railway.app/
2. 点击 "Start a New Project"
3. 选择 "Sign up with Email"（使用邮箱注册）
4. 填写邮箱和密码完成注册

### 创建数据库
1. 登录后点击 "New Project" → "Empty Project"
2. 点击 "+ New" → "Database" → "Add MySQL"
3. 等待创建完成（1-2分钟）
4. 在 "Variables" 标签查看连接信息

### 连接字符串格式
```
jdbc:mysql://MYSQL_HOST:MYSQL_PORT/MYSQLDATABASE?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
```

---

## 方案二：Render PostgreSQL（需要改代码）

### 优点
- ✅ 完全免费
- ✅ 邮箱注册
- ✅ 自动备份

### 缺点
- ❌ 使用PostgreSQL，需要修改代码（将MySQL改为PostgreSQL）
- ⚠️ 免费版有休眠机制（15分钟无活动后休眠）

### 注册
- 访问：https://render.com/
- 使用邮箱注册

---

## 方案三：Supabase（PostgreSQL）

### 优点
- ✅ 完全免费（500MB数据库）
- ✅ 邮箱注册
- ✅ 功能强大（实时、存储等）

### 缺点
- ❌ 使用PostgreSQL，需要修改代码

### 注册
- 访问：https://supabase.com/
- 使用邮箱注册

---

## 方案四：国内云服务（如果Railway不可用）

### 腾讯云
- 学生认证后可以申请免费MySQL
- 访问：https://cloud.tencent.com/

### 阿里云
- 学生认证后可以申请免费RDS MySQL
- 访问：https://www.aliyun.com/

### UCloud
- 新用户有免费试用额度
- 访问：https://www.ucloud.cn/

---

## 推荐使用顺序

1. **首选**：Railway MySQL（最简单，无需改代码）
2. **备选**：如果Railway不可用，使用国内云服务
3. **最后**：如果必须用PostgreSQL，选择Render或Supabase

---

## 快速迁移指南

### 从PlanetScale迁移到Railway

1. **在Railway创建MySQL数据库**
2. **导出PlanetScale数据**（如果有数据）
3. **更新后端配置**：
   - 修改 `application.yml` 或环境变量
   - 更新数据库连接字符串
4. **重新部署后端**

### 如果使用PostgreSQL（需要改代码）

1. **修改pom.xml**：
   ```xml
   <!-- 删除MySQL依赖 -->
   <!-- 添加PostgreSQL依赖 -->
   <dependency>
       <groupId>org.postgresql</groupId>
       <artifactId>postgresql</artifactId>
   </dependency>
   ```

2. **修改application.yml**：
   ```yaml
   spring:
     datasource:
       driver-class-name: org.postgresql.Driver
       url: jdbc:postgresql://host:port/database
   ```

3. **修改SQL语句**（PostgreSQL语法略有不同）

---

## 总结

**对于你的项目，强烈推荐使用Railway MySQL**，因为：
- 不需要GitHub授权
- 不需要修改代码
- 完全免费
- 访问速度快
- 操作简单

按照 `QUICK_START.md` 中的步骤2，使用Railway替代PlanetScale即可！

