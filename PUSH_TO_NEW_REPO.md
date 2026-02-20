# 推送代码到新GitHub仓库

## 📋 操作步骤

### 方法一：使用新账号的Personal Access Token

1. **登录新GitHub账号** (wodeduzihaoe)
2. **创建Personal Access Token**：
   - 访问：https://github.com/settings/tokens
   - 点击 "Generate new token" → "Generate new token (classic)"
   - 勾选 `repo` 权限
   - 生成并复制Token

3. **使用Token推送**：
   ```bash
   git remote set-url new-origin https://YOUR_NEW_TOKEN@github.com/wodeduzihaoe/blog.git
   git push new-origin main
   ```

### 方法二：使用GitHub Desktop或网页上传

如果命令行推送失败，可以：
1. 在GitHub网页上直接上传文件
2. 或使用GitHub Desktop客户端

### 方法三：重新配置远程仓库

```bash
# 删除旧的new-origin
git remote remove new-origin

# 添加新远程仓库（使用Token）
git remote add new-origin https://YOUR_TOKEN@github.com/wodeduzihaoe/blog.git

# 推送
git push new-origin main
```

