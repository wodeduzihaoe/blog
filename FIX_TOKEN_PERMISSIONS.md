# 修复 Fine-grained Token 权限问题

你创建的是 **Fine-grained token**，需要为特定仓库配置权限。

## 解决步骤

### 1. 检查Token权限

1. 访问：https://github.com/settings/personal-access-tokens
2. 找到你的 `blog-push-token`
3. 点击Token名称进入详情页

### 2. 配置仓库权限

在Token详情页面：

1. **Repository access** 部分：
   - 选择 **"Only select repositories"**
   - 在下拉列表中选择 **`wyx1223221/blog`**

2. **Repository permissions** 部分，确保以下权限设置为 **Read and write**：
   - ✅ **Contents** - Read and write
   - ✅ **Metadata** - Read-only（自动）
   - ✅ **Pull requests** - Read and write（如果需要）
   - ✅ **Issues** - Read and write（如果需要）

3. 点击 **"Save"** 保存更改

### 3. 重新推送

配置好权限后，重新执行推送：

```powershell
git push -u origin main
```

---

## 或者：创建 Classic Token（更简单）

如果Fine-grained token配置复杂，可以创建Classic token：

1. 访问：https://github.com/settings/tokens
2. 点击 **"Tokens (classic)"** 标签
3. 点击 **"Generate new token"** → **"Generate new token (classic)"**
4. 填写：
   - **Note**: `blog-push-classic`
   - **Expiration**: 选择过期时间
   - **Select scopes**: 勾选 **`repo`**（这会自动勾选所有repo权限）
5. 点击 **"Generate token"**
6. 复制新Token，然后执行：

```powershell
git remote set-url origin https://NEW_CLASSIC_TOKEN@github.com/wyx1223221/blog.git
git push -u origin main
```

---

## 如果还是不行

如果账号被标记导致无法推送，可能需要：

1. **联系GitHub支持**：https://support.github.com/
2. **检查账号状态**：确保没有违反服务条款
3. **等待标记解除**：某些标记会自动解除

