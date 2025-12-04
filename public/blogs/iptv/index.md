

【白嫖党的史诗级胜利】把Github变成了7x24小时全自动电视台！保姆级教程，实现IPTV源无限续杯！ 

# 🚀 欢迎来到本期的技术分享 —— 自动化IPTV直播源！

- 列表项目

各位同学，大家好。我是 **一万**。欢迎来到本期的技术分享。

相信很多同学都遇到过寻找和维护IPTV直播源的困扰：源地址时常失效，手动验证和更新的过程既繁琐又低效。而我们今天要做的，正是将这一整套重复性劳动，交由程序自动化处理。

这个过程不仅能一劳永逸地解决观看IPTV的需求，更重要的是，它是一个绝佳的学习案例。通过它，你将掌握如何利用`Github`的免费资源，实现任务的定时触发、代码的自动构建与部署。这套技能，可以平移到个人项目、自动化测试、博客部署等多种场景中，具有非常高的实用价值。

接下来，请准备好你的`Github`账号，跟随我的步骤，我们一起从零开始，构建这套属于你自己的自动化系统。

------

## 🛠️ 动手实践：构建你的自动化IPTV系统

我们本次实践的核心，是基于一个优秀的开源项目。首先，我们需要将这个项目引入到我们自己的工作空间。

请打开浏览器，访问`Github`。在搜索栏中输入项目地址：https://github.com/Guovin/iptv-api（地址在视频说明栏） 。我们要找的，是 **IPTV-API** 这个项目。在此，我们首先要向这位项目的开源作者表示感谢 🙏。看完视频如果你觉得作者的这个项目不错，不妨给作者点亮Star。

### 1\. 🚀 Fork 项目仓库

进入项目主页后，我们的第一步操作，是点击页面右上角的 “Fork” 按钮。在`Github`中，“Fork”操作的含义，是将目标仓库完整地复制一份到你自己的账号下，包括所有的代码、分支和提交历史。

创建完成后，页面会自动跳转到 `你的用户名/iptv-api`。现在，这个仓库已经成为我们接下来所有操作的平台。

我们可以打开作者的教程链接。作者的教程写的非常的详细。我们按照他的教程来一步步打造属于我们自己的IPTV直播源。

我们需要修改的地方有三处。

### 2\. 📝 个性化配置：精细化你的直播源

### 2.1 修改 `config/demo.txt` 模板文件

第一步：我们先来点击 `config` 文件夹内 `demo.txt` 模板文件。

您可以复制并参考默认模板的格式进行后续操作。

`config` 文件夹内创建个人模板 `user_demo.txt`。

1. 点击 `config` 目录
2. 创建文件
3. 模板文件命名为 `user_demo.txt`
4. 将复制的模板文件按照需要自行修改。

```
📺央视频道,#genre#
CCTV-1
CCTV-2
CCTV-3
CCTV-4
CCTV-5
CCTV-5+
CCTV-6
CCTV-7
CCTV-8
CCTV-9
CCTV-10
CCTV-11
CCTV-12
CCTV-13
CCTV-14
CCTV-15
CCTV-16
CCTV-17

📡卫视频道,#genre#
广东卫视
浙江卫视
湖南卫视
北京卫视
湖北卫视
黑龙江卫视
安徽卫视
重庆卫视
东方卫视
东南卫视
甘肃卫视
广西卫视
贵州卫视
海南卫视
河北卫视
河南卫视
吉林卫视
江苏卫视
江西卫视
辽宁卫视
内蒙古卫视
宁夏卫视
青海卫视
山东卫视
山西卫视
陕西卫视
四川卫视
深圳卫视
三沙卫视
天津卫视
西藏卫视
新疆卫视
云南卫视
```

1. 点击 `Commit changes...` 进行保存

### 2.2 新建 `config/user_config.ini` 配置文件

第二步：`config` 文件夹内新建个人配置文件 `user_config.ini`。

1. 创建文件
2. 配置文件命名为 `user_config.ini`
3. 粘贴推荐配置

```
[Settings]
#修改模板和结果文件配置：
source_file = config/user_demo.txt
final_file = output/user_result.txt

# 基础功能配置
open_driver = False
open_epg = True
open_empty_category = False
open_filter_resolution = True
open_filter_speed = True
open_speed_test = True
open_update = True
open_update_time = True
open_service = True

# 源功能配置
open_hotel = True
open_hotel_foodie = True
open_hotel_fofa = False
open_local = True
open_multicast = True
open_multicast_foodie = True
open_multicast_fofa = False
open_online_search = False
open_subscribe = True

# 高级功能配置
open_m3u_result = True
open_rtmp = False
open_request = False
open_supply = True
open_use_cache = True
open_history = True
open_headers = False
open_url_info = True

# 网络配置（适合Docker）
#app_host = <http://0.0.0.0>
#app_port = 8000
#cdn_url =

# 数量控制配置
hotel_num = 15
local_num = 15
multicast_num = 15
subscribe_num = 15
online_search_num = 0
urls_limit = 8

# 分页配置
hotel_page_num = 2
multicast_page_num = 2
online_search_page_num = 1

# 地区配置
hotel_region_list = 全部
multicast_region_list = 全部

# 网络类型配置
ipv_type = 全部
ipv_type_prefer = ipv4
ipv4_num =
ipv6_num =
ipv6_support = True

# 过滤条件配置
isp =
location =
min_resolution = 1280x720
max_resolution = 1920x1080
min_speed = 1.0

# 性能优化配置
recent_days = 15
request_timeout = 15
speed_test_limit = 8
speed_test_timeout = 12
speed_test_filter_host = True

# 来源偏好配置
origin_type_prefer = local,subscribe,hotel,multicast

# 时间配置
time_zone = Asia/Shanghai
update_interval = 6
update_time_position = top
```

1. 点击 `Commit changes...` 进行保存

这里我们先不对配置做解释，大家可自行查看 `config.ini` 中的配置解释。

如果你有自己的订阅地址可以填写在 `config/subscribe.txt` 目录下。没有可不填。

如果你有自己的本地源地址可以填写在 `config/local.txt` 目录下。没有可不填。

如果你有需要过滤的接口不希望被收集，请填写在 `config/blacklist.txt` 目录下。比如最近频繁故障的（*肥羊*），我们可以先暂时屏蔽它。

如果你有自己的组播源地址可以填写在 `config/rtp` 目录下。没有可不填。

### 2.3 修改 `Github Actions` 工作流频率

第三步：修改工作流更新频率。

1. 进入 `.github/workflows/main.yml` 目录文件中。
2. 修改修改更新频率 `schedule` 字段。
3. 将定时规则修改为：

```
- cron: '0 10 */2 * *'
```

建议不要太频繁，两天一次就可以。太频繁容易`Actions`功能被封禁。我有前车之鉴。

1. 点击 `Commit changes...` 进行保存

------

## ⚙️ 运行与获取你的专属直播源

需要修改的地方就这么多，修改好之后就可以在`Actions`中手动触发一次了。

由于 Fork 的仓库工作流是默认关闭的，我们点击`Actions`\--`Update schedule`，点击`Enable workflow`按钮确认开启。

这个时候就可以运行更新工作流了，点击`Run workflow`

稍等片刻，就可以看到您的第一条更新工作流已经在运行了！大约等待20分钟。你就可以看到该条工作流已经执行成功。

你可以通过这里 `output/user_result.m3u` 找到你自己的订阅地址。点击`Raw`并复制弹出的链接地址。这里就是属于你自己的IPTV订阅地址了。

------

## 📺 轻松观看：如何使用你的IPTV直播源

当然看到这里你仍一头雾水。那么请你忘记这些步骤。一万也为你写了一条公益的订阅地址。访问：[iptv.910501.xyz](http://iptv.910501.xyz/) 稍微为大家演示如何使用。

好了，至此我们所有的准备工作都已经完成了。

不知道大家平时用哪种播放器来观看iptv呢。

我比较习惯使用TVBox。那接下来我就以TVBox为例演示如何使用我们自己定制的IPTV播放源。

### 1\. TVBox 配置演示

为了方便录制。我以虚拟机为例。

我们打开安装好的TVBox。（如果你需要TVBox可以去我网盘下载，网盘地址：[alist.910501.xyz](http://alist.910501.xyz/)）

打开右上角设置。点击配置地址。在直播源一栏填写我们自己的直播源地址。

**⚠️ 注意**：你如果刚才并没有部署直播源，那么建议你使用我给大家部署的公益地址。\\

https://iptv.910501.xyz

填写时注意填写完整。

数据源地址我们随便写一个。http://ok321.top/tv

保存并退出。

我们点击直播。就可以观看我们刚地址的直播源了。是不是非常方便。

这里不对直播源质量做保证。仅以此项目作为我们了解并掌握如何利用`Github`，实现任务的定时触发、代码的自动构建与部署。

多说一句，对于观影，我更推荐安卓平台使用OrionTV。并使用我给大家部署的公益API地址。我们讲moontv那期视频中有讲。我实际感觉体验会更好。

------

## 📚 学习总结与展望

好的，我们来系统地回顾一下。今天我们基于 [Guovin/iptv-api](https://github.com/Guovin/iptv-api) 项目。从Fork仓库开始，学习了如何进行个性化配置。并最终成功构建和使用了我们自己的自动化IPTV-API系统。

希望大家通过这个案例，不仅掌握了一个实用的工具，更能深刻理解`Github Actions`的强大功能。

感谢各位的观看。如果你认为本期内容对你有所帮助，期待你的一键三连。也欢迎大家关注我的频道，获取更多前沿、深入的技术分享。

实践中遇到的问题，欢迎在评论区交流。那么，本期视频到此结束，我们下期再见。

本文转自 https://blog.910501.xyz/index.php/archives/38/，如有侵权，请联系删除。