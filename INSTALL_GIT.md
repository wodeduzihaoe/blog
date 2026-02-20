# Git 安装指南（Windows）

## 方法一：直接下载安装（推荐）

### 步骤1：下载Git

1. 访问Git官网：https://git-scm.com/download/win
2. 点击下载按钮，会自动下载最新版本的Git安装程序
3. 或者直接访问：https://github.com/git-for-windows/git/releases/latest

### 步骤2：安装Git

1. 双击下载的安装程序（例如：`Git-2.xx.x-64-bit.exe`）
2. 安装过程中：
   - 点击 "Next" 使用默认设置
   - **重要**：在 "Choosing the default editor" 页面，可以选择你喜欢的编辑器（默认是Vim，建议选择VS Code或Notepad++）
   - 在 "Adjusting your PATH environment" 页面，选择 **"Git from the command line and also from 3rd-party software"**（这是默认选项，保持即可）
   - 其他选项保持默认，一直点击 "Next"
   - 最后点击 "Install" 开始安装

### 步骤3：验证安装

安装完成后，**关闭并重新打开PowerShell**，然后执行：

```powershell
git --version
```

如果显示版本号（例如：`git version 2.xx.x`），说明安装成功！

---

## 方法二：使用Chocolatey（如果已安装）

如果你已经安装了Chocolatey包管理器，可以执行：

```powershell
choco install git -y
```

---

## 安装后配置Git

安装完成后，需要配置Git用户信息：

```powershell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"
```

例如：
```powershell
git config --global user.name "yixian"
git config --global user.email "yixian@example.com"
```

---

## 快速安装脚本

如果你想快速安装，可以：

1. 打开浏览器，访问：https://git-scm.com/download/win
2. 下载安装程序
3. 运行安装程序，全部选择默认选项
4. 安装完成后，重新打开PowerShell

---

## 安装完成后

安装完成后，回到项目目录，执行：

```powershell
cd C:\Users\yixian\Desktop\blog
git init
git add .
git commit -m "Initial commit"
```

然后按照 `QUICK_START.md` 的步骤继续部署！

