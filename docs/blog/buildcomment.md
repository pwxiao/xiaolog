---
comments: true
---
# 给你的MKDOCS搭建评论系统

## Giscus
通过 [theme extension]我们可以轻松的给站点添加第三方评论系统. 在这里，我们使用[Giscus], 他基于Github，以
discussions作为后端
  [Giscus]: https://giscus.app/

### Giscus配置

在使用之前 [Giscus], 我们需要完成以下步骤

1.  __安装 [Giscus GitHub App]__ 并连接到Github的存储库，用来存放评论。
    注意这个存储库可以不同于你存放文档的存储库.
2.  __访问 [Giscus] __ 并保存由此产生的代码片段用来加载评论系
    统. 复制这个代码片段，后面会用到. 大概长下面这个样子:

    ``` html
    <script
      src="https://giscus.app/client.js"
      data-repo="<username>/<repository>"
      data-repo-id="..."
      data-category="..."
      data-category-id="..."
      data-mapping="pathname"
      data-reactions-enabled="1"
      data-emit-metadata="1"
      data-theme="light"
      data-lang="en"
      crossorigin="anonymous"
      async
    >
    </script>
    ```

 [`comments.html`][comments] 是默认存放这个代码的地方. 根据 [theme extension] 的提示
 然后添加以下代码:

``` html hl_lines="3"
{% if page.meta.comments %}
  <h2 id="__comments">{{ lang.t("meta.comments") }}</h2>
  <!--在这里存放你保存的代码片段 -->

  <script>
    var giscus = document.querySelector("script[src*=giscus]")
    var palette = __md_get("__palette")
    if (palette && typeof palette.color === "object") {
      var theme = palette.color.scheme === "slate" ? "dark" : "light"
      giscus.setAttribute("data-theme", theme) // (1)!
    }
    document.addEventListener("DOMContentLoaded", function() {
      var ref = document.querySelector("[data-md-component=palette]")
      ref.addEventListener("change", function() {
        var palette = __md_get("__palette")
        if (palette && typeof palette.color === "object") {
          var theme = palette.color.scheme === "slate" ? "dark" : "light"
          var frame = document.querySelector(".giscus-frame")
          frame.contentWindow.postMessage(
            { giscus: { setConfig: { theme } } },
            "https://giscus.app"
          )
        }
      })
    })
  </script>
{% endif %}
```
???+ note "注意"
    这个代码块为我们实现了评论系统主题根据网站主题自动切换

 接下来，我们就只需要在你的每篇文章的头文件中加入以下代码，就可以使用评论了

``` yaml
---
comments: true
---

# 文章标题
...
```

  [Giscus GitHub App]: https://github.com/apps/giscus
  [theme extension]: ../customization.md#extending-the-theme
  [comments]: https://github.com/squidfunk/mkdocs-material/blob/master/src/partials/comments.html
  [overriding partials]: ../customization.md#overriding-partials
  [built-in meta plugin]: ../reference/index.md#built-in-meta-plugin