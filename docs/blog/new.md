
!!! info 前言
    由于之前搭建的hexo博客被我rm掉了，之前写的内容随之而去。这次用mkdocs重新搭建了一个，他更快速，也更符合我的审美。
!!! 思路
    一样的的思路 基于Github+Mkdocs实现



## 搭建过程
### 开始

这里使用 [MkDocs]的Material主题, 我们可以使用 [`pip`][pip]命令来安装 [`mkdocs-material`] .

### 安装
```sh
pip install mkdocs
```
### 预览
```sh
mkdocs serve
```
### 构建
```sh
mkdocs build
```
至此，mkdocs的基本安装就结束了

### 发布
!!! Tips
    首先得配置好Git与远程仓库的连接，常规操作
```sh
mkdocs gh-deploy
```
??? bug "注意"
    以下的文件修改都指的是在[`.mkdocs.yml`]中进行
## 美化
!!! Tips
    初始化的mkdocs界面非常复古，不符合我的审美，因此有必要对界面进行美化，mkdocs的丰富插件让这个工作变得容易不少

### 设置夜间主题
```yaml
theme:
  palette: 

    # Palette toggle for light mode
    - scheme: default
      toggle:
        icon: material/brightness-7 
        name: Switch to dark mode

    # Palette toggle for dark mode
    - scheme: slate
      toggle:
        icon: material/brightness-4
        name: Switch to light mode

```

### 设置字体

```yaml
theme:
  font:
    code: Roboto Mono
```
### 设置图标

```yaml
theme:
  logo: assets/logo.png
  favicon: images/favicon.png
extra:
  homepage: https://example.com
```
### 设置导航栏
导航栏的样式多种多样，可以实现可种各样的效果

以下为我的配置文件

```yaml
theme:
  features:
    - navigation.instant
    - navigation.tracking
    - navigation.tabs
    - navigation.tabs
    - navigation.tabs.sticky
    - navigation.sections
    - navigation.indexes
    - toc.integrate
    - navigation.top
```

### 添加Markdown插件

```yaml
markdown_extensions:
  - admonition
  - abbr
```
