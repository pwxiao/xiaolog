在前后端开发中，我们经常需要在前端页面像服务器请求数据，有时会出现`blocked:mixed-content`报错

打开控制台console可能看到以下内容

![img](https://picx.zhimg.com/80/v2-f3a90e6a30f511e5507cb66d2d0fb848_1440w.png?source=d16d100b)





添加图片注释，不超过 140 字（可选）

### 这是由于我们在https页面请求服务器的http内容，许多浏览器会阻止这种不安全的请求（经测试chrome，edge，safari均会阻止，国产浏览器如QQ，夸克等则选择允许）

### 什么是混合内容（Mixed Content）？

混合内容是指在同一页面中同时包含安全（HTTPS）和非安全（HTTP）资源的情况。当浏览器试图加载非安全资源时，它会发出“混合内容”警告，阻止加载不安全的请求

### 为什么会出现“blocked:mixed-content”错误？

出现这个错误的原因是，您的前端应用可能正在尝试加载一个HTTP资源，而该资源应该通过HTTPS协议进行传输。由于HTTP协议是不安全的，它可能会被中间人攻击（Man-in-the-Middle Attack）拦截，导致数据泄露或恶意修改。因此，浏览器默认阻止了这种不安全的请求。

在我最近的一个课程表项目中就遇到这种问题，网上的解决办法有如下

1. 服务器升级https，
2. 在前端服务器中设置nginx反向代理
3. 在前端html中添加

```
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

但是上述解决方案并不适用于我这个项目，

1.我只有服务器并没有绑定域名，如果是对ip升级https的话，购买ssl证书相对域名来说会很贵

2.我的前端使用Vercel部署，无法配置nginx反向代理（或许是我暂时未发现解决办法）

3.我的服务端数据仅能使用http访问，强制转为http则无法获得内容

在一番思考之后，我想到了另一种解决方案

### 用重新用Vercel部署一个API,首先通过get请求服务器内容，再通过API返回。

```
@app.route('/api/base', methods=['GET'])
def hello_world():
    username = request.args.get('username')
    password = request.args.get('password')
    return requests.get(f"http://xxxxxx:5000/api/base?username={username}&password={password}").text
```

这里使用的是Flask框架部署的api，当然也可以Fastapi等框架。

有一点需要注意，在http站点直接请求这个api时，也会引发CORS跨域问题，但这个在Flask中的解决比较简单

我们需要添加以下代码

```
from flask_cors import CORS
app = Flask(__name__)
CORS(app)
```

这样我们在前端直接请求Vercel api的内容，因为Vercel部署api默认强制https访问，因此便不会出现Mixed Content错误