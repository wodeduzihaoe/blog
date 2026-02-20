# Railway MySQL 变量映射说明

## ✅ 重要：这些变量不需要修改！

Railway自动生成的变量都是正确的，**你不需要修改任何值**。

---

## 📋 从Railway变量到后端配置的映射

根据你看到的Railway变量，你需要提取以下值：

### Railway变量 → 后端需要的值

| Railway变量名 | 你看到的值 | 后端配置用途 |
|--------------|----------|------------|
| `MYSQLHOST` | `mysql.railway.internal` | 数据库主机地址 |
| `MYSQLPORT` | `3306` | 数据库端口 |
| `MYSQLDATABASE` | `railway` | 数据库名称 |
| `MYSQLUSER` | `root` | 数据库用户名 |
| `MYSQLPASSWORD` | `spIFevsdIsgEfwCEPWCfnY0jTILYPDOI` | 数据库密码 |

---

## 🔧 配置后端时如何使用

### 方法一：使用内部地址（推荐，在同一Railway项目中）

如果你的后端也部署在Railway的同一个项目中，使用：

```
SPRING_DATASOURCE_URL=jdbc:mysql://mysql.railway.internal:3306/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTILYPDOI
```

**注意**：`mysql.railway.internal` 只能在Railway内部网络中使用。

### 方法二：使用公共地址（如果后端在其他地方）

如果后端部署在其他平台（如Vercel、其他服务器），需要使用公共地址：

从 `MYSQL_PUBLIC_URL` 中提取：
- Host: `switchback.proxy.rlwy.net`
- Port: `34066`
- Database: `railway`
- User: `root`
- Password: `spIFevsdIsgEfwCEPWCfnY0jTI1YPDOI`

配置：
```
SPRING_DATASOURCE_URL=jdbc:mysql://switchback.proxy.rlwy.net:34066/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTI1YPDOI
```

---

## 📝 你当前的值（从图片中提取）

根据你显示的变量，你的配置应该是：

### 如果后端也在Railway（推荐）：
```
SPRING_DATASOURCE_URL=jdbc:mysql://mysql.railway.internal:3306/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTILYPDOI
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
```

### 如果后端在其他平台：
```
SPRING_DATASOURCE_URL=jdbc:mysql://switchback.proxy.rlwy.net:34066/railway?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=spIFevsdIsgEfwCEPWCfnY0jTI1YPDOI
SPRING_DATASOURCE_DRIVER_CLASS_NAME=com.mysql.cj.jdbc.Driver
```

---

## ⚠️ 重要提示

1. **不要修改Railway的变量**：这些是自动生成的，修改可能导致数据库无法连接
2. **密码安全**：这些密码是敏感信息，不要分享给他人
3. **内部 vs 公共地址**：
   - `mysql.railway.internal` - 只能在Railway内部使用
   - `switchback.proxy.rlwy.net` - 可以从外部访问

---

## 🎯 下一步

1. **保存这些信息**（特别是密码）
2. **创建数据表**（在Database标签中执行SQL）
3. **部署后端**（在Railway中创建后端服务，配置环境变量）

按照 `QUICK_START.md` 的步骤继续操作即可！

