# 免费白嫖Cloudflare算力部署SD画图 无限次数批量生成 多个模型

## 🎯 准备条件

- **Cloudflare账号**

## ✨ 核心优势

- 🆓 **完全免费白嫖**
- 🤖 **支持多个AI模型**
- 📸 **可批量生成8张照片**
- ♾️ **不限制生成次数**
- 📥 **支持批量下载**
- **算力大于5080显卡**

## 🚀 部署步骤

### 第一步：获取源代码

1. 打开GitHub项目页面：[点击访问项目](https://github.com/zhumengkang/cf-ai-image)
2. **如果觉得有用请点个⭐Star支持一下**
3. 打开 `worker.js` 文件并**复制全部代码**
4. 

```
![image-llaD.png](<https://www.zmkk.fun/upload/image-llaD.png>)
```

### 第二步：创建Cloudflare Worker

1. 登录Cloudflare控制台
2. 进入 **Workers** → **Workers和Pages**
3. 

```
![fc11dfab3e99d06101d7c742fefc1ee1.png](<https://www.zmkk.fun/upload/fc11dfab3e99d06101d7c742fefc1ee1.png>)
```

1. 点击 **新建** → 选择 **从 Hello World! 开始**

![ce498ee0e59c356675df7a4911af7331.png](https://www.zmkk.fun/upload/ce498ee0e59c356675df7a4911af7331.png)

1. 输入Worker名称
2. 

```
![8fe6c689c80109e15156f44a957e8d71.png](<https://www.zmkk.fun/upload/8fe6c689c80109e15156f44a957e8d71.png>)
```

1. 点击 **部署** 等待完成

### 第三步：部署代码

1. 点击右上角 **编辑代码**
2. 

```
![3927509c7bc188b9af9102bfe8134d64.png](<https://www.zmkk.fun/upload/3927509c7bc188b9af9102bfe8134d64.png>)
```

1. **Ctrl+A** 全选删除默认代码
2. **Ctrl+V** 粘贴刚才复制的GitHub代码

![image-tewk.png](https://www.zmkk.fun/upload/image-tewk.png)

1. **（可选）修改密码**：编辑第75行的 `password` 字段，默认密码为 `admin123`

### 第四步：添加HTML文件

1. 在代码编辑器空白处右键
2. 

```
![a27dd88a7f857a46395613541d8e3992.png](<https://www.zmkk.fun/upload/a27dd88a7f857a46395613541d8e3992.png>)
```

1. 选择 **新建文件** → 命名为 `index.html`
2. 返回GitHub复制 `index.html` 内容

![2a9b46e99b676ac8084a9bcd529ba952.png](https://www.zmkk.fun/upload/2a9b46e99b676ac8084a9bcd529ba952.png)

1. 粘贴到CF页面中

![image-yjNB.png](https://www.zmkk.fun/upload/image-yjNB.png)

1. 点击右上角 **部署** 并等待完成

### 第五步：绑定AI服务

1. 返回上一页选择 **绑定**
2. 

```
![474e9691c13c2e32ea3f6ad83c45fb73.png](<https://www.zmkk.fun/upload/474e9691c13c2e32ea3f6ad83c45fb73.png>)
```

1. 点击右上角 **添加绑定**

![474e9691c13c2e32ea3f6ad83c45fb73-iUnj.png](https://www.zmkk.fun/upload/474e9691c13c2e32ea3f6ad83c45fb73-iUnj.png)

1. 选择 **Workers AI**

![image-CbQl.png](https://www.zmkk.fun/upload/image-CbQl.png)

1. 输入变量名：`AI`

![image-fwPs.png](https://www.zmkk.fun/upload/image-fwPs.png)

1. 点击 **添加绑定**

## 🎨 开始使用

1. 点击右上角 **访问** 进入应用
2. 

```
![image-ChqG.png](<https://www.zmkk.fun/upload/image-ChqG.png>)
```

1. 输入密码（默认：`admin123`）
2. 输入提示词开始生成

### 示例提示词

```
masterpiece, best quality, amazing quality, very aesthetic, high resolution, ultra-detailed, absurdres, newest, scenery, anime, anime coloring, (dappled sunlight:1.2), rim light, backlit, dramatic shadow, 1girl, long blonde hair, blue eyes, shiny eyes, parted lips, medium breasts, puffy sleeve white dress, forest, flowers, white butterfly, looking at viewer
```

本文转自 https://www.zmkk.fun/archives/1755522674348，如有侵权，请联系删除。