> 没有VPS？教你零成本在Render上部署CLIProxyAPI
>
> 
>
> 由于该系列教程篇幅较长，因此我按主题拆分，大家可以点击目录快速跳转到感兴趣的篇章
>
> - [手把手带你用上AI神器 - CLIProxyAPI（零：配置详细解说）](https://linux.do/t/topic/1011966)
> - [手把手带你用上AI神器 - CLIProxyAPI（壹：项目介绍+Qwen实战）](https://linux.do/t/topic/1011983)
> - [手把手带你用上AI神器 - CLIProxyAPI（贰：Gemini CLI+Codex实战）](https://linux.do/t/topic/1011986)
> - [手把手带你用上AI神器 - CLIProxyAPI（叁：NanoBanana实战）](https://linux.do/t/topic/1011988)
> - [手把手带你用上AI神器 - CLIProxyAPI（肆：中转转发接入篇）](https://linux.do/t/topic/1011999)
> - [手把手带你用上AI神器 - CLIProxyAPI（伍：Docker服务器部署）](https://linux.do/t/topic/1012017)
> - [手把手带你用上AI神器 - CLIProxyAPI（陆：新人最爱GUI）](https://linux.do/t/topic/1033149)
> - [没有VPS？教你零成本在ClawCloud上部署CLIProxyAPI](https://linux.do/t/topic/1033205)
> - [没有VPS？教你零成本在Render上部署CLIProxyAPI](https://linux.do/t/topic/1036951)
> - [没有VPS？教你零成本在Railway上部署CLIProxyAPI](https://linux.do/t/topic/1051621)
> - [没有VPS？教你零成本在HuggingFace上部署CLIProxyAPI](https://linux.do/t/topic/1053666)

在昨天的文章《[没有VPS？教你零成本在ClawCloud上部署CLIProxyAPI](https://linux.do/t/topic/1033205)》发布后，我接着测试了 Render 平台，发现其免费计划不包含持久化存储。在将此情况反馈给 CLIProxyAPI 的作者后，他连夜更新了版本，新增了通过 Git 进行持久化存储的功能。这样一来，我们便能将配置文件和认证文件保存在 GitHub 的私有仓库中，不需要依赖容器云的持久化存储了。

接下来，本文将一步步地指导你如何在没有持久化存储的容器服务（例如 Render 的免费计划）上部署 CLIProxyAPI。至于通过 EasyCLI 进行 OAuth 认证的部分，与在 ClawCloud 上的部署完全一致，请参阅前一篇文章。

### 一、Github 准备工作

首先，我们需要在 GitHub 上创建一个空的仓库。仓库名称可自定义，但**务必**设为**私有**，否则你添加的 API Key 等敏感信息就会裸奔了

![img](https://linux.do/uploads/default/optimized/4X/2/3/2/23237b6948432c39380c6bb99852b97b14afbb5a_2_451x499.png)

764×846 39.9 KB

创建仓库后，记下仓库的 URL 地址。接下来我们点击页面右上角的个人头像，进入 **Settings**，然后点击左侧菜单最下方的 **Developer Settings**

![img](https://linux.do/uploads/default/optimized/4X/6/2/3/623346f06255543ad180e6286ef23fef71f8b922_2_393x500.png)

1037×1317 90.8 KB

接着，依次点击 **Personal access tokens** → **Fine-grained tokens**，然后点击右上角的 **Generate new token**

![img](https://linux.do/uploads/default/optimized/4X/e/9/8/e98bbd8514b1b30fbf8654a0e165812e9144c703_2_690x170.png)

1037×256 14.2 KB

如图所示填写 **Token name**（可自定义），根据你的需求选择过期时间（**Expiration**），并在 **Repository access** 中选择 **Only select repositories**，然后选中我们刚刚创建的那个空白仓库

![img](https://linux.do/uploads/default/optimized/4X/8/6/7/867aeaef4fbd6ea7f5b5bc86f23595930b523037_2_488x500.png)

1037×1061 55.5 KB

将页面向下拉动，在 **Permissions** → **Add permissions** 中找到 **Contents**，添加并将其权限从 `Read-only` 修改为 `Read and write`

![img](https://linux.do/uploads/default/original/4X/4/9/a/49a668ea359bfb92e3bdea32af3cd104fe3b2997.png)

708×509 22.4 KB

确认权限设置无误后，点击页面底部的 **Generate token**

![img](https://linux.do/uploads/default/original/4X/c/d/1/cd15daed9460611008853a829a9cab9e56b079e2.png)

638×333 16.2 KB

此时页面上会显示生成的 Token。请注意，**此 Token 仅会显示一次**，页面关闭后将无法查看，请务必复制并妥善保存

![img](https://linux.do/uploads/default/optimized/4X/a/d/b/adb434e155ad5d34b7930d810af907dd790babbe_2_690x293.png)

1022×435 24 KB

至此，GitHub 的准备工作就全部完成了。

### 二、Render 部署

首先，请确保你已注册 Render 账户。登录后，新建项目并选择 **New Web Service**

![img](https://linux.do/uploads/default/optimized/4X/e/7/1/e717887f6018819b331c5600bcdced2e50c0f630_2_690x458.png)

1134×753 62.1 KB

在部署方式中选择 **Existing Image**，在 **Image URL** 中输入 `eceasy/cli-proxy-api:latest`，然后点击 **Connect**

![img](https://linux.do/uploads/default/optimized/4X/1/c/2/1c2d0ec198a7dde827d73730797103a671205ebf_2_690x308.png)

1134×507 27.6 KB

输入服务名称（**Name**，可自定义），选择区域（**Region**，可根据个人偏好选择），并确保实例类型为 **Free**

![img](https://linux.do/uploads/default/optimized/4X/8/2/7/82735f79a3c455ae2eb9682909ed87fb38ff8ec0_2_651x500.png)

1117×857 42.9 KB

接下来，我们需要添加 4 个环境变量：

- `GITSTORE_GIT_URL`: 你的 GitHub 仓库地址
- `GITSTORE_GIT_USERNAME`: 你的 GitHub 用户名
- `GITSTORE_GIT_TOKEN`: 你刚刚创建的 Personal Access Token
- `MANAGEMENT_PASSWORD`: 用于登录管理界面的密码

输入完成后，点击页面底部的 **Deploy Web Service**

![img](https://linux.do/uploads/default/optimized/4X/d/3/0/d30d38bf19a6fa34b8be7b288744ff6f9893adcb_2_690x415.png)

1034×622 21.2 KB

等待部署日志滚动，当状态变为 **Live** 时，并且日志中出现`Available at your primary URL：XXXX`之后，程序就成功启动了

![img](https://linux.do/uploads/default/optimized/4X/e/5/1/e518f24adf157f64b6b427ac2ce0ad542949dd20_2_430x500.png)

1134×1316 135 KB

使用 Render 提供的 URL，在其后添加 `/management.html`，即可进入 WebUI。输入你设定的 `MANAGEMENT_PASSWORD` 即可登录

![img](https://linux.do/uploads/default/optimized/4X/7/c/b/7cb0c1c29b5ee7046162d893115a9e943f196bd5_2_623x500.png)

1078×865 43.7 KB

此时再查看你的 GitHub 仓库，会发现里边已自动生成了两个文件夹

![img](https://linux.do/uploads/default/optimized/4X/b/3/4/b3413888439e0979c0fd5af912f8535bdd4060a8_2_690x184.png)

1134×303 22.3 KB

至此，在 Render 上部署 CLIProxyAPI 的全流程已完成。其他类似的容器云平台也可采用此方法进行部署，大家可自行探索。

### 三、注意事项

1. CLIProxyAPI 在 v6.2.2 之后才新增了这个功能，如果你想指定镜像版本的话，选择的版本至少为`eceasy/cli-proxy-api:v6.2.2`。
2. 使用此方式部署后，配置文件中的 `remote-management` 部分将不再生效，管理密码以环境变量为准。这意味着，若要修改管理密码，你需要直接修改环境变量 `MANAGEMENT_PASSWORD`。
3. 使用 GitHub 存储配置文件和认证文件，不代表可以在多个容器实例中同时共享和调用，请务必避免这种情况，以免发生冲突。
4. 请注意，当容器正在运行时，直接在 GitHub 仓库中所做的任何手动更改都将是无效的。如确需手动修改，请务必先停止容器服务。
5. 推荐使用 WebUI 或 EasyCLI 来管理配置。使用 EasyCLI 还能进行 OAuth 的远程认证，具体方法可参考本文开头提到的《[没有VPS？教你零成本在ClawCloud上部署CLIProxyAPI](https://linux.do/t/topic/1033205)》中的相关内容。







> 由于该系列教程篇幅较长，因此我按主题拆分，大家可以点击目录快速跳转到感兴趣的篇章
>
> - [手把手带你用上AI神器 - CLIProxyAPI（零：配置详细解说）](https://linux.do/t/topic/1011966)
> - [手把手带你用上AI神器 - CLIProxyAPI（壹：项目介绍+Qwen实战）](https://linux.do/t/topic/1011983)
> - [手把手带你用上AI神器 - CLIProxyAPI（贰：Gemini CLI+Codex实战）](https://linux.do/t/topic/1011986)
> - [手把手带你用上AI神器 - CLIProxyAPI（叁：NanoBanana实战）](https://linux.do/t/topic/1011988)
> - [手把手带你用上AI神器 - CLIProxyAPI（肆：中转转发接入篇）](https://linux.do/t/topic/1011999)
> - [手把手带你用上AI神器 - CLIProxyAPI（伍：Docker服务器部署）](https://linux.do/t/topic/1012017)
> - [手把手带你用上AI神器 - CLIProxyAPI（陆：新人最爱GUI）](https://linux.do/t/topic/1033149)
> - [没有VPS？教你零成本在ClawCloud上部署CLIProxyAPI](https://linux.do/t/topic/1033205)
> - [没有VPS？教你零成本在Render上部署CLIProxyAPI](https://linux.do/t/topic/1036951)
> - [没有VPS？教你零成本在Railway上部署CLIProxyAPI](https://linux.do/t/topic/1051621)
> - [没有VPS？教你零成本在HuggingFace上部署CLIProxyAPI](https://linux.do/t/topic/1053666)

前段时间，我发了一篇文章《[手把手带你用上AI神器 - CLIProxyAPI（伍：Docker服务器部署）](https://linux.do/t/topic/1012017)》，许多网友反馈由于没有VPS，希望我能提供一个在容器云上部署的教程。

实际上，`CLIProxyAPI` 既然支持 Docker 部署，自然也能无缝地在容器云上运行。但若将其直接运行在容器云上，会存在以下两个主要问题：

- **配置文件持久化**：对于程序启动所需的配置文件，容器云往往是通过把配置文件内容映射到特定文件来解决，虽然可以运行，但如果你对配置文件做过修改，一旦容器重启，所有变更都会丢失。这种配置丢失的情况是我们不能接受的。
- **OAuth 认证复杂**：对于需要 OAuth 认证的供应商，在 VPS 的 Docker 环境下，我们可以通过 SSH 隧道将认证回调结果转发到服务器上。而纯容器云环境通常不支持 SSH 隧道，需要通过添加多个端口并在回调时手动修改域名来完成，整个过程非常繁琐。

因此，在 `CLIProxyAPI` 对容器云的部署进行适配更新后，本教程将一步步指导你如何在容器云上完成部署。

本次教程演示使用的容器云平台是 [ClawCloud Run](https://run.claw.cloud/)。在该平台使用注册时长超过180天的 Github 账号登录，即可获得每月5美元的循环额度。我们部署的 `CLIProxyAPI` 每天仅消耗约0.05美元，这个额度绰绰有余。其他容器云平台基本上大同小异，请参考该流程自行部署。

登录 ClawCloud Run 之后，我们点击 **App Launchpad**

![img](https://linux.do/uploads/default/optimized/4X/d/7/b/d7b32a740f1f390d16750b1a09744e34818936b3_2_690x387.png)

1153×648 66.4 KB

点击 **Create APP**

![img](https://linux.do/uploads/default/optimized/4X/4/4/7/44734dfc677ae876ec01595ad425408d0eaee32d_2_690x387.png)

1153×648 43.7 KB

首先我们填写基础信息

- **应用名称 (Application Name)**：可自定义，此处填写 `cliproxyapi`
- **镜像名称 (Image Name)**：`eceasy/cli-proxy-api:latest`
- **网络 (Network)**：容器端口修改为 `8317`，同时打开 **Public Access**

![img](https://linux.do/uploads/default/optimized/4X/0/5/a/05a3ee387ba43ef3e1bb11c22bbd686605dac173_2_527x500.png)

1153×1093 54.9 KB

页面向下拉动，在高级设置中，我们需要填写：

- **启动命令 (Command)**：`/CLIProxyAPI/CLIProxyAPI --config /data/config.yaml`
- **环境变量 (Environment Variables)**：`DEPLOY=cloud`
- **持久化存储 (Local Storage)**：`/data`

![img](https://linux.do/uploads/default/optimized/4X/1/5/4/1545f3990e4e5fc79d5293de4025b45856c36dfb_2_690x476.png)

812×561 22.3 KB

环境变量和存储的填写方法参看下图

![img](https://linux.do/uploads/default/original/4X/a/a/5/aa56ed3c4bf0a5a46ef5d9ae3c6425a790d07767.png)

517×347 6.7 KB

![img](https://linux.do/uploads/default/original/4X/d/4/b/d4bd9c1a84e644cb4a3444f569bcaba7731f20f3.png)

443×306 7.21 KB

确认所有信息填写无误后，点击右上角的 **Deploy Application**，应用将开始部署

![img](https://linux.do/uploads/default/optimized/4X/6/a/5/6a55fd175580450746509d7013b9e517ca896e00_2_690x275.png)

1153×460 15 KB

稍等片刻，应用即可部署成功。当 **Public Address** 状态变为 **Available** 时，其对应的就是我们访问 `CLIProxyAPI` 的网址，请保存备用

![img](https://linux.do/uploads/default/optimized/4X/5/f/e/5fe3519d6d45de5b954106c853644888db27d867_2_512x500.png)

1153×1124 53.2 KB

在等待部署的过程中，我们可以先准备 `config.yaml` 配置文件。本次使用的示例如下，请注意：`remote-management.secret-key` 是远程管理的密钥，而 `api-keys` 是 AI 客户端连接 `CLIProxyAPI` 所使用的密钥，要注意区分

```
port: 8317
remote-management:
  allow-remote: true
  secret-key: "ABCD-1234"
  disable-control-panel: false
auth-dir: "/data/auths"
debug: false
logging-to-file: false
usage-statistics-enabled: false
request-retry: 3
quota-exceeded:
   switch-project: true
   switch-preview-model: true
api-keys:
  - "EFGH-5678"
```

当容器状态变为 **Active** 之后

![img](https://linux.do/uploads/default/optimized/4X/a/a/7/aa7683c8c2b370b1816ef645e070914c7f1878e4_2_690x154.png)

1007×225 11.7 KB

我们点击图中的按钮，打开之前添加的 **Local Storage**

![img](https://linux.do/uploads/default/optimized/4X/6/2/7/6270f53940cf52e0f24b10c32090227bb32710ed_2_690x154.png)

1007×225 11.9 KB

点击右上角的 **Upload**，选择刚才准备好的 `config.yaml` 文件并上传

![img](https://linux.do/uploads/default/optimized/4X/a/8/b/a8ba76e2402d3cd42d38de62c750f28d9ea39a49_2_690x227.png)

1074×354 17.8 KB

上传完成后，点击 **Restart** 重启容器

![img](https://linux.do/uploads/default/optimized/4X/d/0/a/d0a87584118d4ddb21974b86a44bc384316a4ca5_2_690x154.png)

1007×225 11.3 KB

稍等片刻，待容器状态再次变为 **Active** 后，我们可以看到 **Local Storage** 中已生成了新的文件

![img](https://linux.do/uploads/default/optimized/4X/6/5/2/652d62edb5e4ad4099971c87e6fb3fb01d2afd90_2_690x312.png)

1068×483 27.3 KB

同时，点击 **Logs** 标签页，可以看到如下图所示的日志信息

![img](https://linux.do/uploads/default/optimized/4X/e/0/d/e0dc900c16481eb8e3a19074c8e2012c288b9f0c_2_690x154.png)

1007×225 11.4 KB

![img](https://linux.do/uploads/default/original/4X/5/0/c/50cd3a78edccb5954cf57c239477b25fe7ffcb2e.png)

1064×363 27.8 KB

至此，`CLIProxyAPI` 便成功完成了整个部署流程。

------

**使用 EasyCLI 进行远程 OAuth 认证**

接下来，我们使用官方的另一个项目 [EasyCLI](https://github.com/router-for-me/EasyCLI) 来进行远程 OAuth 添加。

`EasyCLI` 是 `CLIProxyAPI` 的配套项目，提供了一个图形用户界面（GUI）来管理 `CLIProxyAPI`。其最大亮点是支持完整的 OAuth 认证授权流程（不仅是上传授权文件，而是能处理整个授权回调过程），这是 `CLIProxyAPI` 自带的 WebUI 无法做到的。

请前往 [EasyCLI 程序发布页面](https://github.com/router-for-me/EasyCLI/releases) 下载适合你操作系统的版本（作者提供了 Mac、Linux、Windows 版本）。本教程以 Windows x64 版本为例。

打开程序后，选择 **Remote**，输入我们之前记录下的 URL 网址

![img](https://linux.do/uploads/default/original/4X/b/1/f/b1f5676b9f988daf6af88f7d78719410cc52b292.png)

532×412 10.7 KB

密码输入 `config.yaml` 中设置的 `remote-management.secret-key`（本例中是 `ABCD-1234`）

依次点击 **Authentication Files** → **New**

![img](https://linux.do/uploads/default/optimized/4X/d/7/1/d711a38be40c683c3feda4df76534e9209174db8_2_690x467.png)

932×632 27.2 KB

本次我们仍以添加 Gemini CLI 为例进行演示，准备工作可参照《[手把手带你用上AI神器 - CLIProxyAPI（贰：Gemini CLI+Codex实战）](https://linux.do/t/topic/1011986)》

输入 **Project ID**，点击 **Confirm**

![img](https://linux.do/uploads/default/optimized/4X/a/e/2/ae26bca8887422e52955fd27bf4792d8b2005652_2_690x467.png)

932×632 26.8 KB

页面中会出现 OAuth 链接，点击 **Open Link**

![img](https://linux.do/uploads/default/optimized/4X/c/9/d/c9dc773d39c0b9a7fd46f4b19c541cb4190119dd_2_690x467.png)

932×632 28.2 KB

程序会自动打开浏览器并跳转到 OAuth 链接，同时 `EasyCLI` 自身会进入回调接收状态

![img](https://linux.do/uploads/default/optimized/4X/7/2/7/727a0f2a862070c400d87ce30c5460a8ad4c5365_2_690x467.png)

932×632 31.6 KB

在打开的浏览器页面中，我们登录账号并完成授权认证过程

![img](https://linux.do/uploads/default/original/4X/1/f/d/1fdf1bcd9d5a19668d675309f4d05a38be4c743f.png)

509×607 17.4 KB

完成后，在 **Authentication Files** 列表中就可以看到新生成的配置文件了

![img](https://linux.do/uploads/default/optimized/4X/a/7/8/a7893fd9c0705c94f357f0469e63fc744a762c19_2_690x467.png)

932×632 22.4 KB

**验证**

我们再用 Cherry Studio 测试一下。如图所示，根据配置文件内容填写 API 密钥和 API 地址

![img](https://linux.do/uploads/default/optimized/4X/1/f/f/1ffce08709a2e8d13a43c1077164044d07672aa6_2_690x439.png)

964×614 23.9 KB

成功！

![img](https://linux.do/uploads/default/original/4X/8/b/6/8b68fceaf573e7ae9687ee1801eb45c2bb89e73e.png)

517×427 10.8 KB

`EasyCLI` 的其余功能就交给各位自行探索了。实际上，除了 OAuth 认证部分，`EasyCLI` 的其他功能与系统内置的 WebUI 基本一致。你也可以通过访问 `https://你的CLIProxyAPI访问链接/management.html` 来进行其他配置管理（关于 WebUI 的介绍可参考这篇文章《[手把手带你用上AI神器 - CLIProxyAPI（陆：新人最爱GUI）](https://linux.do/t/topic/1033149)》，虽然介绍也相对简短=。=）