# GitHub 推送代码指南（账号被标记情况）

由于你的GitHub账号被标记，无法使用OAuth授权，需要使用Personal Access Token来推送代码。

## 步骤1：创建Personal Access Token

1. **访问GitHub设置**
   - 登录GitHub
   - 点击右上角头像 → **Settings**
   - 左侧菜单 → **Developer settings**
   - 点击 **Personal access tokens** → **Tokens (classic)**

2. **生成新Token**
   - 点击 **Generate new token** → **Generate new token (classic)**
   - 填写信息：
     - **Note**: `blog-push-token`（随便写个名字）
     - **Expiration**: 选择过期时间（建议90天或自定义）
     - **Select scopes**: 勾选 **`repo`**（这会自动勾选所有repo相关权限）
   - 滚动到底部，点击 **Generate token**

3. **复制Token**
   - ⚠️ **重要**：Token只显示一次，立即复制保存！
   - 格式类似：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## 步骤2：使用Token推送代码

### 方法一：在URL中包含Token（推荐）

在PowerShell中执行（替换 `YOUR_TOKEN` 为你的Token）：

```powershell
git remote set-url origin https://YOUR_TOKEN@github.com/wyx1223221/blog.git
git push -u origin main
```

**示例**（不要直接复制，替换YOUR_TOKEN）：
```powershell
git remote set-url origin https://ghp_xxxxxxxxxxxx@github.com/wyx1223221/blog.git
git push -u origin main
```

### 方法二：使用Git Credential Manager

1. 推送时会提示输入用户名和密码：
   - **Username**: 输入你的GitHub用户名 `wyx1223221`
   - **Password**: 输入你的Personal Access Token（不是GitHub密码）

2. 执行推送：
```powershell
git push -u origin main
```

### 方法三：使用环境变量（临时）

```powershell
$env:GIT_ASKPASS = "echo"
git -c credential.helper='!f() { echo "username=wyx1223221"; echo "password=YOUR_TOKEN"; }; f' push -u origin main
```

## 步骤3：验证推送成功

推送成功后，访问 https://github.com/wyx1223221/blog 查看你的代码。

## 解决账号被标记问题

如果账号被标记，可能需要：

1. **联系GitHub支持**
   - 访问：https://support.github.com/
   - 说明情况，请求解除标记

2. **检查账号活动**
   - 确保没有违反GitHub服务条款
   - 检查是否有异常活动

3. **等待自动解除**
   - 某些标记可能会自动解除

## 安全提示

⚠️ **重要安全提示**：
- Personal Access Token相当于密码，不要分享给他人
- 不要在公开场合（如截图、代码）暴露Token
- Token过期后需要重新生成
- 如果Token泄露，立即在GitHub设置中删除它

## 后续推送

设置好Token后，后续推送可以直接使用：
```powershell
git push
```

如果需要更新Token，重新执行步骤2的方法一即可。

