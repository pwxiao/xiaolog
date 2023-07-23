# 基于Vercel和Flask部署API

项目地址：[https://github.com/pwxiao/MyWords_API](https://github.com/pwxiao/MyWords_API)

## 引言

前段时间无意间发现了Vercel这个平台，它可以免费部署服务器和托管网站，与GithubPages只能托管静态网站不同，vercel可以部署的内容有很多，它支持python，go等多语言环境，这意味你可以创建动态的内容，甚至，你可以在上面部署一个深度学习模型。

## 简介
Vercel有一个Serverless Function 功能，翻译过来就是`无服务器函数`,因此，通过这个功能，我们可以在Vercel部署我们的api，我们今天这个项目就是基于Vercel部署的。

这个项目名字叫做MyWords_API,通过对服务器进行`GET`请求，你会得到随机返回的一个句子，重新请求，句子也会随之刷新。

为了更直观的体验，你可以点击[https://api.pwxiao.top/](https://api.pwxiao.top/)。

另外这个api还支持更多的功能，具体操作请查看项目的[README.md](https://github.com/pwxiao/MyWords_API)文件。

## 开发过程中遇到的坑

### 多路由

我们可能会设置不同的路由已使用不同的api接口，但是官方的路由只支持对主页进行请求
，对此我们只需要修改源代码中的vercel.json文件
将默认的vercel.json修改为
```json
{
    "rewrites": [
        { "source": "/(.*)", "destination": "/api/app" }
    ]
}
```
!!! info "注意"
    `destination`中的app，为你创建的flask_app的名称，按需修改

### 文件读取
在函数如果涉及到文件读取，我试过通过文件的open()函数来读取，这种方法在本地调试时是可行的，但是部署到vercel端时会出现`500`错误，暂时不知错误的原因，解决办法就是通过部署到vercel的文件的网络路径，使用requests.get()获取源代码，这样也能获取到文件的内容

```python
url=requests.get("https://api.pwxiao.top/sentences/category.json")
text = url.text
res = json.loads(text)
```

### json返回中文乱码

在默认情况下，vercel会将内容转化为ASCII码输出，这样获取的内容只需要通过转码就可以了，但是在浏览器访问时不太美观，例如下面这样

```json
{
    "id": 1082,
    "uuid": "ab89b12e-359b-4cfe-b50d-de35a20bf928",
    "hitokoto": "\u4f17\u91cc\u5bfb\u4ed6\u5343\u767e\u5ea6\uff0c\u84e6\u7136\u56de\u9996\uff0c\u90a3\u4eba\u5374\u5728\uff0c\u706f\u706b\u9611\u73ca\u5904\u3002",
    "type": "i",
    "from": "\u9752\u7389\u6848%b7\u5143\u5915",
    "from_who": null,
    "creator": "131",
    "creator_uid": 146,
    "reviewer": 0,
    "commit_from": "web",
    "created_at": "1481559051",
    "length": 24
}
```
我们需要他显示成这样
```json
  {
    "id": 1082,
    "uuid": "ab89b12e-359b-4cfe-b50d-de35a20bf928",
    "hitokoto": "众里寻他千百度，蓦然回首，那人却在，灯火阑珊处。",
    "type": "i",
    "from": "青玉案·元夕",
    "from_who": null,
    "creator": "131",
    "creator_uid": 146,
    "reviewer": 0,
    "commit_from": "web",
    "created_at": "1481559051",
    "length": 24
  }
```
这时只需要将原内容进行一次json.dumps()转换并声明`ensure_ascii=False`

```python
import json
fina_res = json.dumps(result,ensure_ascii=False)

```

## API的使用与部署

该API分为两个功能

### 随机生成一句

请求地址：

```url
https://api.pwxiao.top/
```

参数说明：*https://api.pwxiao.top?catogory=*
```
’Anime':动画,
'Comic':漫画,
'Game':游戏,
'Literature':文学,
'Original':原创,
'Internet':来源网络,
'Other':其他,
'Video':影视,
'Poem':古诗文,
'NCM':网易云,
'Philosophy':哲学,
'Funny':搞笑
```
>数据库来自[一言数据库](https://github.com/hitokoto-osc/sentences-bundle)

返回值：
```
{
    "id": 1263,
    "uuid": "a58936ac-f3fb-4e5d-838e-a77a7219d1a4",
    "hitokoto": "渔舟唱晚，响穷彭蠡之滨；雁阵惊寒，声断衡阳之浦。",
    "type": "i",
    "from": "滕王阁序",
    "from_who": null,
    "creator": "毛毛毛布斯",
    "creator_uid": 562,
    "reviewer": 0,
    "commit_from": "web",
    "created_at": "1507184994",
    "length": 24
  }
```

### 搜索古诗

请求地址:

```url
https://api.pwxiao.top/search/
```

参数说明：*https://api.pwxiao.top/search/?s=query*
```
query为要搜索的内容
```







### 使用Vercel一键部署

&emsp;&emsp;点击下方按钮根据提示操作即可

&emsp;&emsp;[![Powered by Vercel](https://www.datocms-assets.com/31049/1618983297-powered-by-vercel.svg)](https://vercel.com/new/clone?repository-url=https://github.com/pwxiao/MyWords_API)









