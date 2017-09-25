---
title: Hexo之Next主题添加 algolia 搜索
date: 2017-09-02 12:16:49
tags: [hexo, 美化]
---

### 1、为什么添加algolia搜索

	是为了可以方便的查找所需文章，提供便利！

  命令安装algolia插件

  ```
  npm install --save hexo-algolia
  ```

### 2、添加效果

	这里效果图就没有了（暂时）

### 3、去algolia 注册自己的账号，完成注册后，创建项目(index_name 注意)，打开打开API Keys页面，里面的信息等会需要写到hexo的配置文件中。

在根目录的站点配置文件_config.yml
中加入如下配置，参照上图的各种key值

``` md 
algolia:
  appId: 'appid'
  apiKey: 'apiKey'
  adminApiKey: 'adminApiKey'
  indexName: '上面填写的index名'
  chunkSize: 5000
  fields:
    - title
    - slug
    - path
    - content:strip
```
在主题Next目录下找到配置文件，修改里面的 

```
local_search:
  enable: true
```


在git bash中执行(注意每次在写新文章时都要提交index)

```
hexo clean

hexo g

hexo d

hexo algolia

```
 如下则表示成功

```
INFO ...
...
INFO ...
```
好了， 现在我们已经将代码提交到algolia里面了，

如果无法提交成功，先执行hexo clean即可。
在\themes\next下找到_config.yml，找到如下内容，将enable修改为true，labels修改为自己需要的

在themes\next\layout_partials中找到header.swig，找到以下代码并修改

``` swig
<nav class="site-nav">
  <!-- 添加  theme.algolia_search.enable -->
  {% set hasSearch = theme.swiftype_key || theme.algolia_search.enable || theme.tinysou_Key || config.search %}


  {% if theme.menu %}
    <ul id="menu" class="menu">
      {% for name, path in theme.menu %}
        {% set itemName = name.toLowerCase() %}
        <li class="menu-item menu-item-{{ itemName }}">
          <a href="{{ url_for(path) }}" rel="section">
            {% if theme.menu_icons.enable %}
              <i class="menu-item-icon fa fa-fw fa-{{theme.menu_icons[itemName] | default('question-circle') | lower }}"></i> 

            {% endif %}
            {{ __('menu.' + itemName) }}
          </a>
        </li>
      {% endfor %}


  {% if hasSearch %}
    <li class="menu-item menu-item-search">
      {% if theme.swiftype_key %}
        <a href="javascript:;" class="st-search-show-outputs">
      {% elseif config.search %}
        <a href="javascript:;" class="popup-trigger">

<!-- 添加 开始 -->


      {% elseif theme.algolia %}
        <a href="javascript:;" class="popup-trigger">

<!-- 添加 结束 -->


      {% endif %}
        {% if theme.menu_icons.enable %}
          <i class="menu-item-icon fa fa-search fa-fw"></i> <br />
        {% endif %}
        {{ __('menu.search') }}
      </a>
    </li>
  {% endif %}
</ul>

  {% endif %}


  {% if hasSearch %}
    <div class="site-search">
      {% include 'search.swig' %}
    </div>
  {% endif %}
</nav>

```

好了， 现在就完成添加了。