**利用 CF 的 Workers 创建无限的免费图床！爽歪歪**

### 利用 CF 的 Workers 创建无限的免费图床是怎样的一种体验？今天我们就来动手试试，看下效果到底如何！

![图片[1]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/1642b4954120251031230231.webp)

### 优点

1. 无限图片储存数量，你可以上传不限数量的图片
2. 无需购买服务器，托管于Cloudflare的网络上，当使用量不超过Cloudflare的免费额度时，完全免费
3. 无需购买域名，可以使用Cloudflare Pages提供的*.pages.dev的免费二级域名，同时也支持绑定自定义域名
4. 支持图片审查API，可根据需要开启，开启后不良图片将自动屏蔽，不再加载
5. 支持后台图片管理，日志管理，查看访问前20的Referer、IP、img,可以对上传的图片进行在线预览，添加白名单，黑名单等操作

### 免费图床开源项目：【[链接直达](https://www.freedidi.com/?golink=aHR0cHM6Ly9naXRodWIuY29tL3gtZHIvdGVsZWdyYXBoLUltYWdl&nonce=7cd34930e1)】

![图片[2]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/b30e14450620251031230325.webp)

 

### 利用Cloudflare pages部署

1. 点击[Use this template](https://www.freedidi.com/?golink=aHR0cHM6Ly9naXRodWIuY29tL3gtZHIvdGVsZWdyYXBoLUltYWdlL2dlbmVyYXRl&nonce=7cd34930e1)按钮创建一个新的代码库。
2. 登录到[Cloudflare](https://www.freedidi.com/?golink=aHR0cHM6Ly9kYXNoLmNsb3VkZmxhcmUuY29tLw==&nonce=7cd34930e1)控制台.
3. 在帐户主页中，选择`pages`> ` Create a project` > `Connect to Git`
4. 选择你创建的项目存储库，在`Set up builds and deployments`部分中，`Framework preset(框架)`选`Next.js`即可。

![图片[3]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/872c79338420251031230402.jpg)

1. 点击`Save and Deploy`部署 。

2. 设置环境变量&开启图片管理功能

   1. 创建D1数据库 如图

   ![图片[4]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/bb6663c2e020251031230426.png)

   ![图片[5]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/723965462920251031230443.png)

   1. 执行sql命令创建表（在控制台输入框粘贴下面语句执行即可）

      DROP TABLE IF EXISTS tgimglog;

      CREATE TABLE IF NOT EXISTS tgimglog (

      `id` integer PRIMARY KEY NOT NULL,

      `url` text,

      `referer` text,

      `ip` varchar(255),

      `time` DATE

      );

      DROP TABLE IF EXISTS imginfo;

      CREATE TABLE IF NOT EXISTS imginfo (

      `id` integer PRIMARY KEY NOT NULL,

      `url` text,

      `referer` text,

      `ip` varchar(255),

      `rating` text,

      `total` integer,

      `time` DATE

      );

       

3. 设置兼容性标志，前往后台依次点击`设置`->`函数`->`兼容性标志`->`配置生产兼容性标志` 填写 `nodejs_compat`

![图片[6]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/fdc1496ea220251031230511-scaled.png)

1. 前往后台点击`部署` 找到最新的一次部署点`重试部署`。

![图片[7]-利用 CF 的 Workers 创建无限的免费图床！爽歪歪-零度博客](https://www.freedidi.com/wp-content/uploads/2025/10/09496f7c5720251031230533-scaled.png)

> 环境变量

| 变量名称              | 值                                                           | type    |
| --------------------- | ------------------------------------------------------------ | ------- |
| PROXYALLIMG           | 反向代理所有图片（默认为false）                              | boolean |
| BASIC_USER            | 后台管理页面登录用户名称                                     | string  |
| BASIC_PASS            | 后台管理页面登录用户密码                                     | string  |
| ENABLE_AUTH_API       | 是否开启访客验证 （默认为false）                             | boolean |
| REGULAR_USER          | 普通用户 （访客验证）                                        | string  |
| REGULAR_PASS          | 普通用户密码                                                 | string  |
| ModerateContentApiKey | 审查图像内容的API key                                        | string  |
| RATINGAPI             | [自建的鉴黄api](https://www.freedidi.com/?golink=aHR0cHM6Ly9naXRodWIuY29tL3gtZHIvbnNmd2pzLWFwaQ==&nonce=7cd34930e1) | string  |
| CUSTOM_DOMAIN         | [https://your-custom-domain.com](https://www.freedidi.com/?golink=aHR0cHM6Ly95b3VyLWN1c3RvbS1kb21haW4uY29tLw==&nonce=7cd34930e1) (自定义加速域名) | string  |
| TG_BOT_TOKEN          | 123468:AAxxxGKrn5 (从 [@BotFather](https://www.freedidi.com/?golink=aHR0cHM6Ly90Lm1lL0JvdEZhdGhlcg==&nonce=7cd34930e1)) | string  |
| TG_CHAT_ID            | -1234567 (频道的ID,TG Bot要是该频道或群组的管理员)           | string  |

> TG_BOT_TOKEN

> 获取ID机器人 [@VersaToolsBot](https://www.freedidi.com/?golink=aHR0cHM6Ly90Lm1lL1ZlcnNhVG9vbHNCb3Q=&nonce=7cd34930e1)

> `TG_CHAT_ID`为目标对话的唯一标`ID`或目标频道的用户名（eg: @channelusername），当目标对话为个人或私有频道是只能是`ID`,当为公开频道或群组是可以为目标频道的用户名（eg: `@channelusername`）



 第1步：检查环境变量是否设置

    1. 登录 https://dash.cloudflare.com/
    2. Workers & Pages → 你的项目
    3. 设置（Settings） → 环境变量（Environment variables）
    4. 检查是否有这两个变量：

| 变量名       | 是否存在？ | 值的格式                                   |
| ------------ | ---------- | ------------------------------------------ |
| TG_BOT_TOKEN | ？         | 应该是：数字:字母数字混合 如 123456:ABCxyz |
| TG_CHAT_ID   | ？         | 应该是：-100开头的负数 或 @频道名          |

  如果没有这两个变量 → 继续第2步添加
  如果有但格式不对 → 继续第2步修改

---

  第2步：重新获取并配置TG环境变量

  2.1 获取正确的 TG_BOT_TOKEN

    1. 打开Telegram，搜索 https://t.me/BotFather
    2. 发送 /mybots
    3. 选择你的Bot
    4. 点击 API Token
    5. 完整复制 Token

  Token格式检查：

  - ✅ 正确：1234567890:ABCdefGHIjklMNOpqrSTUvwxYZ123456
  - ❌ 错误：只有数字，或只有字母，或缺少冒号

---

  2.2 获取正确的 TG_CHAT_ID

  方法A：使用机器人获取（最准确）

    1. 把你的Bot添加到目标频道（如果还没添加）
    2. 在频道发一条消息（任意内容）
    3. 打开浏览器，访问：

    https://api.telegram.org/bot你的BOT_TOKEN/getUpdates

    3. 把 你的BOT_TOKEN 替换成真实Token
    4. 找到返回结果里的 "chat":{"id":-1001234567890} 这样的内容
    5. 复制这个负数ID（包括负号）

  方法B：使用 @VersaToolsBot

    1. 搜索 https://t.me/VersaToolsBot
    2. 点击 Start
    3. 把Bot返回的你的User ID记下来
    4. 如果要频道ID，需要在频道里发消息后Bot才能获取

  方法C：使用公开频道用户名

  - 如果频道是公开频道（有设置频道用户名）
  - 直接用 @频道用户名（带@符号）

---

  2.3 在Cloudflare设置环境变量

    1. 还是在 环境变量（Environment variables） 页面
    2. 点击 编辑变量（Edit variables） 或 添加变量（Add variable）
    3. 添加或修改这两个变量：

  变量名：TG_BOT_TOKEN
  值：1234567890:ABCdefGHIjklMNOpqrSTUvwxYZ123456

  变量名：TG_CHAT_ID
  值：-1001234567890

  ⚠️ 重要检查：

  - 变量名完全一致（大小写敏感）
  - Token 完整复制（有冒号，前后都有内容）
  - CHAT_ID 格式正确（负数或@频道名）
  - 没有多余空格

    4. 点击 保存（Save）

---

  第3步：确保Bot是频道管理员

  这一步非常tm重要！

    1. 打开你的Telegram频道
    2. 点击频道名 → 管理员（Administrators） 或 Admins
    3. 检查Bot是否在管理员列表里

  如果Bot不在列表：

    1. 点击 添加管理员（Add Administrator）
    2. 搜索你的Bot用户名（在BotFather里可以看到，通常是 @你的bot名_bot）
    3. 添加Bot
    4. 给Bot权限（至少勾选）：

    - ✅ 发送消息（Post Messages） ← 必须！
    - ✅ 编辑消息（Edit Messages）（可选）

    5. 点击 保存（Save）

---

  第4步：测试Bot和频道连接

  在浏览器访问这个URL（测试Bot能否发送消息）：

  https://api.telegram.org/bot你的BOT_TOKEN/sendMessage?chat_id=你的CHAT_ID&text=测试

  记得替换：

  - 你的BOT_TOKEN → 真实的Token
  - 你的CHAT_ID → 真实的频道ID或@用户名

  成功的返回：
  {
    "ok": true,
    "result": {
      "message_id": 123,
      ...
    }
  }

  失败的返回：
  {
    "ok": false,
    "error_code": 400,
    "description": "Bad Request: chat not found"  // ← CHAT_ID错误
  }
  或
  {
    "ok": false,
    "error_code": 401,
    "description": "Unauthorized"  // ← BOT_TOKEN错误
  }
  或
  {
    "ok": false,
    "description": "Forbidden: bot is not a member of the channel"  // ← Bot不是管理员
  }

  根据返回结果：

  - ✅ "ok": true → 配置正确！继续第5步
  - ❌ "ok": false → 根据错误信息修改配置，重新测试

---

  第5步：重新部署项目

  修改了环境变量后必须重新部署！

    1. 回到Cloudflare Pages项目
    2. 点击 部署（Deployments）
    3. 找到最新的部署
    4. 点击 ··· → 重试部署（Retry deployment）
    5. 等待部署完成（1-3分钟）

---

  第6步：测试上传功能

    1. 访问：https://picbed-2o5.pages.dev
    2. 选择一张图片
    3. 选择 TG Channel 上传
    4. 点击上传

  成功标志：

  - ✅ 图片出现在你的TG频道里
  - ✅ 返回一个可访问的URL
  - ✅ 没有 500 错误

---

  📋 快速诊断清单

  按顺序检查这些：

    1. TG_BOT_TOKEN 已设置且格式正确（有冒号，前后都有字符）
    2. TG_CHAT_ID 已设置且格式正确（负数或@频道名）
    3. Bot已添加到频道并且是管理员
    4. 用浏览器测试 /sendMessage API 返回 "ok": true
    5. 修改环境变量后已重新部署
    6. /api/total 返回正常（D1数据库正常）





 第1步：确认你在正确的数据库

  看看你控制台页面顶部，应该有显示当前数据库名称，截图给老王看看！

  或者告诉我：

  - 你现在在哪个数据库里？（数据库名叫什么？）
  - Pages项目绑定的数据库叫什么？

---

  第2步：一条一条执行SQL（别tm着急！）

  清空输入框，然后只粘贴第一条：

  DROP TABLE IF EXISTS tgimglog;

  点击 执行（Execute） → 看到成功提示

---

  然后清空输入框，粘贴第二条：

  DROP TABLE IF EXISTS imginfo;

  点击 执行（Execute） → 看到成功提示

---

  然后清空输入框，粘贴第三条：

  CREATE TABLE tgimglog (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      url TEXT,
      referer TEXT,
      ip TEXT,
      time TEXT
  );

  点击 执行（Execute） → 看清楚结果！有没有报错？

---

  然后清空输入框，粘贴第四条：

  CREATE TABLE imginfo (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      url TEXT,
      referer TEXT,
      ip TEXT,
      rating TEXT,
      total INTEGER,
      time TEXT
  );

  点击 执行（Execute） → 看清楚结果！有没有报错？

---

  第3步：立即验证

  清空输入框，粘贴：

  SELECT name FROM sqlite_master WHERE type='table';

  点击 执行（Execute）

  必须看到：
  name
  _cf_KV
  tgimglog
  imginfo