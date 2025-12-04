# [夸客网盘搜索网站-基于pansou和心悦搜索微改，AI让小白实现技术平权](https://linux.do/t/topic/889555)

**先给结论：**
夸客网盘搜索：https://www.kuakeso.net/

# 前言

**1. 喝水不忘挖井人**
致敬pansou作者和心悦搜索作者

![img](https://linux.do/user_avatar/linux.do/fish2018/48/623516_2.png)

[【PanSou】高性能网盘搜索，集成TG、网盘搜索插件，异步更新缓存](https://linux.do/t/topic/780754) [资源荟萃](https://linux.do/c/resource/14)

> 来都来了，点个star再走？ 一个简约版的前端页面(集成API文档，可在线调试接口)： 更新 ![white_check_mark](https://linux.do/images/emoji/twemoji/white_check_mark.png?v=14) 202508071317 优化全局缓冲区初始化策略 退出程序时强制保存内存缓存数据到磁盘 ![white_check_mark](https://linux.do/images/emoji/twemoji/white_check_mark.png?v=14) 202508051129 TG频道搜索支持匹配图片，返回images字段，想要海报的可以尝试下 …

[github.com](https://github.com/675061370/xinyue-search)

![img](https://linux.do/uploads/default/optimized/4X/0/8/5/085abdc610574d1edc28ceee7e187cff48da8ae3_2_690x344.png)

### [GitHub - 675061370/xinyue-search: 心悦搜索-支持多网盘转存分享，支持夸克网盘、百度网盘、阿里云盘、UC网盘，打造高效便捷的网盘资源搜索...](https://github.com/675061370/xinyue-search)

心悦搜索-支持多网盘转存分享，支持夸克网盘、百度网盘、阿里云盘、UC网盘，打造高效便捷的网盘资源搜索引擎！

**2. AI实现技术平权ing**
本人技术小白（高中理科生大学读工业设计现在干销售），借助vscode+Claude 4花费了4个周末，网站搭起来了。

> PS：以下内容对于技术大佬们，属于关公面前耍大刀/班门弄斧了

# 网站部署

```
网站采用心悦搜索前后端+pansou API搜索服务
```

**我的采购清单**

1. 域名 1个 82元 spaceship
2. 香港服务器4H4G 99元2年 狐*云
3. CDN亚太高级版1T永久 300元 狐*云CNM
   小计：481元

## 网站部署历程

- pansou API部署，[@fish2018](https://linux.do/u/fish2018) 大佬已经写的很详细了，我跟着教程做的，没有遇到什么问题。有问题去帖子下面留言
- 前后端部署，这是一波三折，好好分享我踩的坑

心悦作者写了一个部署文档，但对于我这种小白的说，会有难度。去B站搜到一个老版本的部署教程，照着做没有太大的问题。(我就不放链接了，那个人是卖源码的。B站搜心悦搜索，就看得到）

### 遇到的第1个困难：安装时，数据库链接不成功

![6576f3e1c46cdddef2326bde97b84eaf](https://linux.do/uploads/default/optimized/4X/3/8/7/3879f166aecee5771757cf0db593545d8dae2518_2_690x226.png)

6576f3e1c46cdddef2326bde97b84eaf1044×342 22.1 KB

服务器地址+端口，把能试的都试过了，还是不行
去心悦群里问大佬们，大佬说1panel面板的数据库加密方式和作者的代码不匹配，需要对数据库做设置。

![d49a17fd295fd1ba9ed8512891748d73](https://linux.do/uploads/default/optimized/4X/5/9/4/594be2f7890663e4a9611702895217c5c64515d8_2_449x375.jpeg)

d49a17fd295fd1ba9ed8512891748d731207×1007 134 KB

听话照做，**依然链接不成功**！第一个周末就一直在搞这个事儿。问了各种AI，答案也是差不多，就是搞不了

**解决方案**
**重装系统，安装宝塔面板，再装php和MySQL，不要用docker装数据库**
（也不懂为什么。如果有大佬知道原因，麻烦告诉下在1panel面板装容器数据库如何链接成功，我还是喜欢1panel面板的UI，感谢）

### 遇到的第2个困难：如何正确使用AI写代码

**最终选用的是vscode+github copilot（试用一个月）**。就光选工具，对于小白来说，也是问了很久AI

![image](https://linux.do/uploads/default/optimized/4X/4/0/4/404c358b9d8dc1aa67acc842cdb0beeddfd3cd1d_2_517x351.png)

image1433×975 116 KB

**AI模型最终选的是Claude4**。选AI这过程，对于小白来说也是很折磨人。
刷抖音，今天听这个博主说这个AI很牛皮，明天又听到那个博主说那个AI又升级了，人类要被淘汰了。
还是毛爷爷那句话是真理：实践出真知！最傻逼是GPT 4,后面有了5，稍微好一点，但还是觉得Claude4好用些

用了大佬的方法，AI好用很多了，不再折磨人了。
**先让他阅读全部代码，让他自己生成一个网站说明文档，然后再告诉他以后的代码修改，必须要基于这个文档来执行**

![image](https://linux.do/uploads/default/optimized/4X/3/f/0/3f0f1ad5401bb2caaddaf4d39f1abadb254dcfaa_2_690x306.jpeg)

image1920×852 115 KB

后面就是一些基础问答、部署、再问答、再部署，反反复复。。。

> 这个过程是最享受的，技术大佬们体是会不到这种快乐

> 一个技术小白，说几句话，就看到屏幕上代码跳过去跳过来，瞬间觉得自己牛皮惨了~像美国大片那样，敲几下代码，代码满屏幕跑~

部分截图

![image](https://linux.do/uploads/default/optimized/4X/1/6/4/1641274f2bf137e9a048018f780efa365a8f39e4_2_565x499.png)

image1055×932 111 KB

![image](https://linux.do/uploads/default/optimized/4X/5/7/4/574a71ffebe2fffb6a23ba8c920ae26e954c185a_2_690x242.png)

image1060×372 33.8 KB

![image](https://linux.do/uploads/default/optimized/4X/d/8/1/d817a88ed2cf45c62aef2cadcc1bbfa8e8166eca_2_690x312.png)

image1059×479 60.8 KB

![image](https://linux.do/uploads/default/optimized/4X/d/0/0/d002190f2fee74796b9755d0fe5d69565a08ad9f_2_690x426.png)

image1062×656 69.9 KB

![image](https://linux.do/uploads/default/optimized/4X/0/3/f/03f7b917231384dd4c6922d14539df5325ed0b41_2_690x282.png)

image1059×434 30.8 KB

![image](https://linux.do/uploads/default/optimized/4X/4/8/c/48ccfcd6b86c8774e3268a7d0bcd1a81daeb8746_2_690x299.png)

image1061×460 40.3 KB

最后这一步，**配置微信机器人，没有成功**。作者说要看缘分，看来是缘分未到。

> 空了一定要把这个功能搞定，觉得很屌~（有大佬指导一下，那就感激不尽了）
>
> ![image](https://linux.do/uploads/default/original/4X/7/6/1/761ff7dbb3e4c1a36a45076d2ba2a6454e80f5d5.png)
>
> image705×266 16.5 KB

### CDN加速访问

```
> 在这之前，也用过很多网盘搜索网站，有2个大的痛点：打开网页慢、广告+验证码
```

> 所以，我想既然投入这么多精力折腾，就一定要折腾一个自己用起来爽的。不然对不起自己的日日夜夜“敲代码”

QQ群里找到一个充了VIP会员要转手的兄弟，比我在官网买便宜，300元永久果断下手，虽然永久是忽悠人的，至少不会有时间焦虑。就当多洗一次脚~而且洗脚只爽90分钟，这个可以爽很久！哈哈

> 域名未备案，买的亚太，速度还可以，大家也可以测试下。顺便测试下他家CDN够不够硬

![image](https://linux.do/uploads/default/optimized/4X/1/6/e/16e25e365c6e0bf42a9483480870dec8a5cfca8a_2_690x345.png)

image1781×893 176 KB

这里再分享个小细节，**AI真的会让技术小白上瘾的**

我想把 [kuakeso.net](http://kuakeso.net/) 重定向到 [www.kuakeso.net](http://www.kuakeso.net/)，从最开始AI给我找了3个方案，后面又衍生出第四个方案，最后半夜1点都上床了，想到第五个方案，问AI可不可行。

确定可行而且效果更好，爬起来搞到2点半~

![image](https://linux.do/uploads/default/optimized/4X/3/9/d/39d07b74a979623579f62f03f18075a7ba9b9ad7_2_690x454.png)

image1433×943 104 KB

在调试CDN过程又学到了，**如何通过CDN保护源站IP**，发现新大陆新知识，来回折腾半天，越干越有劲~

![image](https://linux.do/uploads/default/optimized/4X/5/9/9/599b6ce3562353859984409135afc317b205e6b9_2_587x500.png)

image1436×1222 127 KB

## 网站后台配置

后台配置，都是一些基础的傻瓜式操作（心悦作者大佬很懂用户体验啊）
不过，也遇到一个困难，就是api接口的配置，也是来回折腾了很久，找到了正确打开方式~很有成就感。

因为用的是pansou API接口，接头暗号和心悦作者有一些出入（心悦作者不提供API接口了）。

需要的佬，夸克网盘接口直接复制就可以

> 百度网盘接口设置方法：把下方"quark"修改为"baidu"（2个地方都需要改哈）

请求头：

```
{
"User-Agent": "Mozilla/5.0",
"Accept": "application/json"
}
```

接口参数

```
{
"kw": "{keyword}",
"res": "merge",
"cloud_types": "quark"
}
```

字段映射

```
{
"list_path": "data.merged_by_type.quark",
"fields": {
"title": "note",
"url": "url",
"password": "password",
"datetime": "datetime"
}
}
```

# 感受体会

差不多了写完了，我靠1点了，写了3个小时。边写边想，找记录搞截图。
但是很爽、很有成就感~

永远想不到，自己也可以写一篇网站搭建的1/5技术贴~
感谢AI时代，感恩国富民强！！！

最重要的是感恩Linux社区！ **真诚** 、**友善** 、**团结** 、**专业** ，共建你我引以为荣之社区。

以前没事的时候刷抖音，现在一空下来就打开CDN面板看访问量

![image](https://linux.do/uploads/default/optimized/4X/5/c/1/5c126af684da5bf32d742dc73be1491810e62fe0_2_690x429.png)

image1394×868 66.4 KB

**最后留个请求**，这个网站还有2个执念没有完成

1. 1panel面板docker数据库链接不成功
2. 微信机器人接口没有配置成功

如果大佬们有解决办法，希望可以不吝赐教~buy coffee表示感谢！

> 这个网站我会继续好好维护下去
> 我为它想了三个关键词：**速度快、无广告、最新资源**

CDN不花钱了，域名续费一年80，服务器续费一年50。
如果一次性再续费个5年，就当多去一次95洗脚

**传送门**
夸客网盘搜索：https://www.kuakeso.net/