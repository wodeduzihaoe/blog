# Railway 邮箱注册指南（解决GitHub授权问题）

由于你的GitHub账号被标记，无法使用GitHub登录Railway，**使用邮箱注册即可**！

## ✅ 解决方案：使用邮箱注册

### 方法一：直接在Railway注册页面使用邮箱

1. **访问Railway注册页面**
   - 访问：https://railway.app/
   - 点击 "Start a New Project" 或 "Get Started"

2. **选择邮箱注册**
   - 在登录/注册页面，**不要点击GitHub按钮**
   - 查找 "Sign up with Email" 或 "Continue with Email" 选项
   - 如果没有看到，点击 "Sign up" 或 "Create account"

3. **填写注册信息**
   - 输入你的邮箱地址
   - 设置密码
   - 完成注册验证（可能需要邮箱验证）

### 方法二：如果页面只显示GitHub登录

如果Railway页面只显示GitHub登录选项，可以尝试：

1. **使用Magic Link（魔法链接）**
   - 在登录页面，查找 "Sign in with Email" 或 "Use Email Instead"
   - 输入邮箱，Railway会发送登录链接到你的邮箱
   - 点击邮箱中的链接即可登录

2. **直接访问邮箱注册链接**
   - 尝试访问：https://railway.app/login
   - 或：https://railway.app/signup
   - 查找邮箱注册选项

### 方法三：使用其他登录方式

Railway可能还支持：
- Google账号登录
- Microsoft账号登录
- 其他OAuth提供商

---

## 🔧 如果还是无法注册

### 方案A：联系Railway支持

1. 访问Railway支持页面
2. 说明你的GitHub账号被标记，无法使用GitHub登录
3. 请求使用邮箱注册

### 方案B：使用其他平台

如果Railway无法注册，可以使用其他免费数据库平台：

1. **Render** - https://render.com/
   - 支持邮箱注册
   - 提供PostgreSQL（需要改代码）或MySQL

2. **Supabase** - https://supabase.com/
   - 支持邮箱注册
   - 提供PostgreSQL（需要改代码）

3. **Fly.io** - https://fly.io/
   - 支持邮箱注册
   - 提供PostgreSQL

---

## 📝 注册成功后的步骤

注册成功后，按照 `QUICK_START.md` 的步骤继续：

1. 创建MySQL数据库
2. 获取连接信息
3. 创建数据表
4. 部署后端

---

## ❓ 常见问题

### Q: 为什么Railway只显示GitHub登录？
A: Railway默认优先显示GitHub登录，但通常也支持邮箱注册。仔细查找页面上的其他登录选项。

### Q: 找不到邮箱注册选项怎么办？
A: 尝试：
1. 滚动页面查看是否有其他选项
2. 点击 "Sign up" 而不是 "Log in"
3. 尝试访问不同的URL（如 /signup）
4. 使用其他浏览器或清除缓存

### Q: 邮箱注册后可以连接GitHub仓库吗？
A: 可以！注册后，在Railway项目设置中可以连接GitHub仓库进行自动部署。

---

## 🎯 推荐操作

**最简单的方法**：
1. 访问 https://railway.app/
2. 点击 "Start a New Project"
3. 如果只看到GitHub登录，尝试：
   - 点击页面上的 "Sign up" 文字链接
   - 或直接输入邮箱地址（有些页面支持直接输入）
   - 或查找 "Continue with Email" 选项

如果还是找不到邮箱注册选项，告诉我，我可以帮你找其他替代方案！

