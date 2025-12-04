# Claude Code Windows使用指南 

## (一) 安装Node.js环境 

### Windows安装方法 

**方法一：官网下载（推荐）**

1. 打开浏览器访问 `https://nodejs.org/`
2. 点击 "LTS" 版本进行下载（版本号要大于18，推荐长期支持版本）
3. 下载完成后双击 `.msi` 文件
4. 按照安装向导完成安装，保持默认设置即可

**方法二：使用包管理器**

如果你安装了 Chocolatey 或 Scoop，可以使用命令行安装：

powershell

```
# 使用 Chocolatey
choco install nodejs
# 或使用 Scoop
scoop install nodejs
```

**Windows 注意事项**

- 建议使用 PowerShell 而不是 CMD
- 如果遇到权限问题，尝试以管理员身份运行
- 某些杀毒软件可能会误报，需要添加白名单

**验证安装是否成功**

安装完成后，打开 PowerShell 或 CMD，输入以下命令：

powershell

```
node --version
npm --version
```

如果显示版本号，说明安装成功了！

------

## (二) 安装 Claude Code 

### 安装 Claude Code 

打开 PowerShell 或 CMD，运行以下命令：

##### 全局安装 Claude Code 

powershell

```
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

这个命令会从 npm 官方仓库下载并安装最新版本的 Claude Code。更新也使用这个命令

**提示**

- 建议使用 PowerShell 而不是 CMD，功能更强大
- 如果遇到权限问题，以管理员身份运行 PowerShell

**验证 Claude Code 安装**

安装完成后，输入以下命令检查是否安装成功：

powershell

```
claude --version
```

如果显示版本号，恭喜你！Claude Code 已经成功安装了。

### 更新 Claude Code 

运行

bash

```
claude update
```

------

## (三) 配置 Claude Code 

### 方法一 (强强强烈推荐!!!) : 使用CC SWITCH进行配置 

使用cc switch 配置 查看 [cc switch](https://docs.88code.org/FAQ/CC_SWITCH.html)

### 方法二（推荐）：通过文件设置 

编辑文件 `~/.claude/settings.json` 文件添加以下内容(如果没有settings.json文件，请自行创建，不需要时可随意删除，不影响claude使用)：

windows下路径为: C:/Users/你的用户名/.claude

Linux 或 macOS 系统中通常位于: ∼/.claude

json

```
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的API密钥",
    "ANTHROPIC_BASE_URL": "https://www.88code.org/api",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  },
  "permissions": {
    "allow": [],
    "deny": []
  }
}
```

### 方法三：通过环境变量设置 

为了让 Claude Code 连接到你的中转服务，需要设置两个环境变量：

**PowerShell 临时设置（当前会话）**

在 PowerShell 中运行以下命令：

powershell

```
$env:ANTHROPIC_BASE_URL = "https://www.88code.org/api"
$env:ANTHROPIC_AUTH_TOKEN = "你的API密钥"
```

NOTE

记得将 "你的API密钥" 替换为在上方 "API Keys" 标签页中创建的实际密钥。

**PowerShell 永久设置（用户级）**

在 PowerShell 中运行以下命令设置用户级环境变量：

powershell

```
# 设置用户级环境变量（永久生效）
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "https://www.88code.org/api", [System.EnvironmentVariableTarget]::User)
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", "你的API密钥", [System.EnvironmentVariableTarget]::User)
```

查看已设置的环境变量：

powershell

```
# 查看用户级环境变量
[System.Environment]::GetEnvironmentVariable("ANTHROPIC_BASE_URL", [System.EnvironmentVariableTarget]::User)
[System.Environment]::GetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", [System.EnvironmentVariableTarget]::User)
```

设置后需要重新打开 PowerShell 窗口才能生效。

**验证环境变量设置**

设置完环境变量后，可以通过以下命令验证是否设置成功：

在 PowerShell 中验证：

powershell

```
echo $env:ANTHROPIC_BASE_URL
echo $env:ANTHROPIC_AUTH_TOKEN
```

在 CMD 中验证：

cmd

```
echo %ANTHROPIC_BASE_URL%
echo %ANTHROPIC_AUTH_TOKEN%
```

**预期输出示例：**

```
https://www.88code.org/api
cr_xxxxxxxxxxxxxxxxxx
```

如果输出为空或显示变量名本身，说明环境变量设置失败，请重新设置。

------

## (四) 开始使用 Claude Code 

**现在你可以开始使用 Claude Code 了！**

启动 Claude Code：

powershell

```
claude
```

在特定项目中使用：

powershell

```
# 进入你的项目目录
cd C:\path\to\your\project
# 启动 Claude Code
claude
```

------

## Windows 安装常见问题解决 

**安装时提示 "permission denied" 错误**

这通常是权限问题，尝试以下解决方法：

- 以管理员身份运行 PowerShell
- 或者配置 npm 使用用户目录：`npm config set prefix %APPDATA%\npm`

**PowerShell 执行策略错误**

如果遇到执行策略限制，运行：

powershell

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**环境变量设置后不生效**

设置永久环境变量后需要：

- 重新启动 PowerShell 或 CMD
- 或者注销并重新登录 Windows
- 验证设置：`echo $env:ANTHROPIC_BASE_URL`