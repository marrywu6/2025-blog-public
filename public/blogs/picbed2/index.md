# 如何利用Cloudflare和Telegram搭建一个免费、无限容量的个人图床

## 引言

对于拥有个人博客或网站的开发者来说，一个稳定、快速且免费的图床是刚需。传统的解决方案要么需要付费，要么有容量或流量限制。本文将介绍一个创新的解决方案，通过结合Cloudflare Pages、D1数据库以及Telegram的存储能力，来搭建一个几乎零成本、无限容量、全球CDN加速的个人图床。

### 方案优势

*   **零成本**：完全基于Cloudflare的免费套餐，无服务器和域名费用。
*   **无限容量**：利用Telegram作为后端存储，图片上传数量无上限。
*   **全球加速**：享受Cloudflare强大的全球CDN网络，图片访问速度快。
*   **带管理后台**：项目自带后台管理页面，可以进行图片预览、日志查看、黑白名单管理等操作。
*   **可选内容审查**：支持接入图片审查API，自动屏蔽不良图片。

---

## 准备工作

在开始之前，请确保您拥有以下账号：

1.  **GitHub 账号**
2.  **Cloudflare 账号**
3.  **Telegram 账号**

---

## 部署步骤

### 第一步：准备Telegram Bot和频道

图床的后端存储依赖于一个Telegram频道，并通过一个Bot来自动上传图片。

1.  **创建Telegram Bot**
    *   在Telegram中搜索 `@BotFather` 并开始对话。
    *   发送 `/newbot` 命令，按照提示为你的Bot设置一个名称（Name）和用户名（Username）。
    *   创建成功后，`@BotFather` 会给你一串 **HTTP API Token**，请务必完整复制并妥善保管，例如 `1234567890:ABCdefGHIjklMNOpqrSTUvwxYZ123456`。

2.  **创建私有频道**
    *   在Telegram中创建一个新的**私有频道**（Private Channel），作为你的图片存储仓库。

3.  **将Bot设为频道管理员**
    *   进入你创建的私有频道，点击频道信息 -> 管理员 -> 添加管理员。
    *   搜索并添加你刚刚创建的Bot。
    *   确保给予Bot**“发送消息 (Post Messages)”**的权限。

4.  **获取频道ID (Chat ID)**
    *   这是最关键的一步。首先，在你的私有频道里随意发送一条消息（例如“hello”）。
    *   然后，将这条消息**转发**给一个能获取ID的机器人，例如 `@VersaToolsBot`。
    *   该机器人会返回一条包含来源频道ID的信息，找到 "Forwarded from channel" 字段，其ID通常是一个以 `-100` 开头的负数，例如 `-1001234567890`。请完整复制这个ID。

### 第二步：Fork项目并部署到Cloudflare Pages

1.  **Fork GitHub仓库**
    *   访问开源项目主页：`https://github.com/x-dr/telegraph-Image`
    *   点击页面右上角的 **“Fork”** 按钮，将项目完整复制到你自己的GitHub账号下。

2.  **连接到Cloudflare Pages**
    *   登录Cloudflare控制台，进入 **Workers & Pages** -> **创建应用程序** -> **Pages** -> **连接到Git**。
    *   选择你刚刚Fork的项目仓库。
    *   在 **“设置构建和部署”** 页面，**框架预设**选择 `Next.js`。
    *   直接点击 **“保存并部署”**，进行首次部署。

### 第三步：创建并绑定D1数据库

图床的管理后台需要一个数据库来存储图片信息和日志。

1. **创建D1数据库**

   *   在Cloudflare控制台，进入 **Workers & Pages** -> **D1** -> **创建数据库**。
   *   为数据库命名（例如 `image-hosting-db`），并选择一个位置。

2. **绑定数据库到Pages项目**

   *   回到你的Pages项目，进入 **设置** -> **函数** -> **D1数据库绑定**。
   *   点击 **“添加绑定”**，变量名称填写 `DB`，D1数据库选择你刚刚创建的那个。

3. **执行SQL创建数据表**

   *   回到D1数据库的管理页面，进入 **“控制台”**。
   *   将以下SQL命令**逐条**粘贴到输入框中并执行，确保每条都成功。

   ```sql
   CREATE TABLE IF NOT EXISTS tgimglog (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       url TEXT,
       referer TEXT,
       ip TEXT,
       time TEXT
   );
   ```

   ```sql
   CREATE TABLE IF NOT EXISTS imginfo (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       url TEXT,
       referer TEXT,
       ip TEXT,
       rating TEXT,
       total INTEGER,
       time TEXT
   );
   ```

   *   执行完毕后，可以运行 `SELECT name FROM sqlite_master WHERE type='table';` 来验证 `tgimglog` 和 `imginfo` 两个表是否已成功创建。

### 第四步：设置环境变量并重新部署

1.  **配置环境变量**
    *   回到Pages项目，进入 **设置** -> **环境变量** -> **生产** -> **添加环境变量**。
    *   至少添加以下两个**必须**的变量：
        *   `TG_BOT_TOKEN`：值为你从 `@BotFather` 获取的API Token。
        *   `TG_CHAT_ID`：值为你获取的频道ID（那个`-100`开头的负数）。
    *   为了保护你的管理后台，强烈建议添加以下两个**可选**变量来设置登录密码：
        *   `BASIC_USER`：后台登录用户名。
        *   `BASIC_PASS`：后台登录密码。

2.  **设置兼容性标志**
    *   在Pages项目的 **设置** -> **函数** -> **兼容性标志** 中，为“生产”环境添加 `nodejs_compat` 标志。

3.  **重新部署**
    *   完成以上所有配置后，进入项目的 **“部署”** 标签页。
    *   找到最新的一次部署记录，点击 **“重试部署”**，以应用刚刚所有的配置。

---

## 如何使用

等待部署成功后，访问Cloudflare提供给你的 `*.pages.dev` 域名，你就可以看到图床的上传界面了。选择图片，点击上传，图片链接会自动生成。

如果你设置了后台登录密码，可以访问 `/admin` 路径进入管理后台。

至此，一个完全免费、功能强大的个人图床就搭建完成了！