---
home: true
comments: true
changelog: True
statistics: true
---

# o(〃'▽'〃)o Hi! :smiling_face_with_3_hearts:

!!! info "网站信息"
    这里是乔不思的个人笔记本哦  

    
    !!! info "网站统计"
        页面总数：{{pages}}  
        总字数：{{words}}  
        代码块行数：{{codes}} 


        网站运行时间：<span id="web-time"></span>



<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?28f18450da302a3ecb5dafd02685a81f";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

<script>
function updateTime() {
    var date = new Date();
    var now = date.getTime();
    var startDate = new Date("2022/10/01 09:10:00");
    var start = startDate.getTime();
    var diff = now - start;
    var y, d, h, m;
    y = Math.floor(diff / (365 * 24 * 3600 * 1000));
    diff -= y * 365 * 24 * 3600 * 1000;
    d = Math.floor(diff / (24 * 3600 * 1000));
    h = Math.floor(diff / (3600 * 1000) % 24);
    m = Math.floor(diff / (60 * 1000) % 60);
    if (y == 0) {
        document.getElementById("web-time").innerHTML = d + "<span class=\"heti-spacing\"> </span>天<span class=\"heti-spacing\"> </span>" + h + "<span class=\"heti-spacing\"> </span>小时<span class=\"heti-spacing\"> </span>" + m + "<span class=\"heti-spacing\"> </span>分钟";
    } else {
        document.getElementById("web-time").innerHTML = y + "<span class=\"heti-spacing\"> </span>年<span class=\"heti-spacing\"> </span>" + d + "<span class=\"heti-spacing\"> </span>天<span class=\"heti-spacing\"> </span>" + h + "<span class=\"heti-spacing\"> </span>小时<span class=\"heti-spacing\"> </span>" + m + "<span class=\"heti-spacing\"> </span>分钟";
    }
    setTimeout(updateTime, 1000 * 60);
}
updateTime();
function toggle_statistics() {
    var statistics = document.getElementById("statistics");
    if (statistics.style.opacity == 0) {
        statistics.style.opacity = 1;
    } else {
        statistics.style.opacity = 0;
    }
}
</script>

```python title="script.py"
if visitor.name == 'pwxiao':
    print(f"看什么看，快去学习{'/'.join(tasks)}\n")
    logging.warning("别摸了")
else:
    print("别来无恙ヽ(*´∀｀)八(´∀｀*)ノ\n")
    thanks_list.append(visitor.name)
```
[最近更新](#){ .md-button }
## 2023

### 9月9日
- [智能零售商品识别](python/lean_in_jixie.md)

### 8月14日
- [小工具上线](python/2023814.md)

### 7月22日
- [本站完美迁移至Vercel](blog/trans_to_vercel.md)
### 7月22日
- [Vercel+Flask部署API](python/vercel_deploy_api.md)
### 6月22日
- [Plotter](blog/plotter.md)
### 6月19日
- [ADB命令大全](Java/adbcodes.md)
### 6月18日
- [校园网自动登录](python/login_net.md)
### 6月7日
- [傅里叶级数证明巴塞尔问题](blog/proof_fourier.md)
### 4月18日
- [常见算法](algorithm/%E5%B8%B8%E8%A7%81%E7%AE%97%E6%B3%95.md)
### 4月4日
- [物体追踪算法](python/ObjectTracker.md)
### 4月2日
- [Java2022.pdf题解](Java/java2022.md)
### 3月29日
- [正则表达式](blog/zzexpression.md)
### 2月23日，2023
- [Opencv学习](blog/opencvlearn.md)

### 1月29日，2023
- [年度总结](blog/2022summary.md)
- [十大经济学原理](blog/econimicsLaw.md)

## 2022
### 12月23日，2022

- [python作业](python/1.md)
 
- [用vector对结构体排序](cpp/c%2B%2B%E8%BF%9B%E9%98%B6.md)

- [利用c++重载实现复数运算](cpp/c%2B%2B%E8%BF%9B%E9%98%B6.md)
### 12月21日，2022
- [c++实现二维数组卷积](algorithm/c%2B%2B%E5%AE%9E%E7%8E%B0%E5%8D%B7%E7%A7%AF.md)

### 12月16日，2022
- 网站支持emoji表情。
- 增加网站统计
- 增加搜索功能
- [Taichi入门](algorithm/Taichi%E5%85%A5%E9%97%A8.md)
- [利用Taichi加速Python](algorithm/%E5%88%A9%E7%94%A8Taichi%E5%8A%A0%E9%80%9FPython.md)
### 12月15日，2022
- [分解质因数](algorithm/%E5%88%86%E8%A7%A3%E8%B4%A8%E5%9B%A0%E6%95%B0.md)
- [万能头文件](cpp/cindex/#_3)
- [为你的MKDOCS搭建评论系统](blog/buildcomment.md)
### 12月14日, 2022 
- [我的作品](work/studybar.md)上线
- [c++读入输出优化](ccpp/cindex/#_1)
- [char-or-string](cpp/cindex/#char-or-string)
### 12月13日, 2022 
- [记一次解谜经历](blog/letitsnow.md)
- 给网站增加了雪花特效
- 网站支持夜间模式
### 12月12日, 2022 
- 新站完工 
- [搭建过程](blog/new.md)
- [洛谷-过河卒](algorithm/%E6%B4%9B%E8%B0%B7-%E8%BF%87%E6%B2%B3%E5%8D%92.md)
