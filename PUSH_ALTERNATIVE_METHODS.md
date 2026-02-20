# 推送代码的替代方法

## ❌ 当前问题

网络连接GitHub失败，可能是：
- 网络问题
- 需要代理
- 防火墙限制

## ✅ 解决方案

### 方法一：使用GitHub网页上传（最简单）

1. **访问新仓库**：https://github.com/wodeduzihaoe/blog
2. **点击 "uploading an existing file"** 或 **"Add file" → "Upload files"**
3. **拖拽整个项目文件夹**到网页
4. **填写提交信息**：`Initial commit`
5. **点击 "Commit changes"**

**注意**：需要上传整个项目，包括 `backend` 和 `frontend` 文件夹

### 方法二：使用GitHub Desktop

1. **下载GitHub Desktop**：https://desktop.github.com/
2. **登录新GitHub账号**（wodeduzihaoe）
3. **添加仓库**：
   - File → Clone Repository
   - 选择 `wodeduzihaoe/blog`
   - 选择本地路径
4. **复制代码**：
   - 将当前项目的所有文件复制到新仓库文件夹
5. **提交并推送**：
   - 在GitHub Desktop中点击 "Commit to main"
   - 点击 "Push origin"

### 方法三：配置Git代理（如果有代理）

如果你有代理，可以配置：

```bash
# 设置HTTP代理
git config --global http.proxy http://proxy.example.com:8080
git config --global https.proxy https://proxy.example.com:8080

# 然后重试推送
git push new-origin main
```

### 方法四：使用SSH方式（如果配置了SSH密钥）

```bash
# 改用SSH URL
git remote set-url new-origin git@github.com:wodeduzihaoe/blog.git
git push new-origin main
```

---

## 🎯 推荐操作

**最简单的方法**：使用GitHub网页上传（方法一）

1. 访问：https://github.com/wodeduzihaoe/blog
2. 点击 "uploading an existing file"
3. 上传整个项目文件夹
4. 提交

---

## 📝 上传后

代码上传成功后，在Railway中：
1. 点击 "+ New" → "GitHub Repo"
2. 使用新GitHub账号（wodeduzihaoe）授权
3. 选择 `wodeduzihaoe/blog` 仓库
4. Root Directory 选择 `backend`
5. 点击 "Deploy"

