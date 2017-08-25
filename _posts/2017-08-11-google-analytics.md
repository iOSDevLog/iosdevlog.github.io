---
layout: post
title: "Use Google Analytics in Jekyll"
description: ""
category: jekyll
tags: [Google]
---

## <https://analytics.google.com/>

![add](/assets/images/Google/Analytics/add.png)

创建一个新的帐号。

![info](/assets/images/Google/Analytics/info.png)

保存 跟踪ID: 

UA-104431384-1

和网站跟踪的 JavaScipt 代码。

## <http://jekyllrb.com/docs/themes/> 里面有关于 Google Analytics 的配置说明。

在 `_config.yml` 里面添加下面2行。

```ruby
# Google Analytics
google_analytics: UA-104431384-1
```

在 `_includes` 下面添加 `google-analytics.html`, 内容为上面保存的 JavaScript 代码。

```javascript
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-104431384-1', 'auto');
  ga('send', 'pageview');

</script>
```

## 生效

可以在 Chrome 上面安装 [Google Analytics（分析）帮助](https://get.google.com/tagassistant),查看Google Analytics是否安装成功！

![enable](/assets/images/Google/Analytics/enable.png)

还没有开启

![notInstalled](/assets/images/Google/Analytics/not_installed.png)

网站没有使用 Google Analytics

![installed](/assets/images/Google/Analytics/installed.png)

网站使用 Google Analytics

![result](/assets/images/Google/Analytics/result.png)

用电脑，手机或者iPad访问，过几分钟就可以看到实时统计效果了。
