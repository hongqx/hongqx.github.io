## Chrome 新的自动播放策略
最近看 来自 Chrome 团队在 IO 2018上的 分享 [《Build awesome media experiences on the web》](https://www.youtube.com/watch?v=5azRhKsSU_M) 。大概主要是说关于 音视频在 Chrome 上的更新。其中业务团队需要及时关注新的 自动播放策略，虽然在去年9月份 Chrome 团队就更新了博客 [《Autoplay Policy Changes》](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)。 Chrome 会在 2018年 的第二个季度，采取新的自动播放策略。

目前该策略在很多场景已经存在，用户初始进入到视频页面的时候，会阻止默认自动播放，如果用户在当前域名下选择了播放，在多次选择之后，就会默认可执行自动播放。目前该策略也对我们线上的一些场景产生了影响，参照之前对safari11以上版本webkit内核根据用户设置阻止视频自动播放的案例的处理，下面是针对chrome上无法自动播放的一些整理，以及在移动web端自动播放支持的情况总结。

### 自动播放的策略调整
![Chrome Autoplay Policy](http://img1.vued.vanthink.cn/vueda728bcacf470e6922f5ba6325af54c81.png)
如图中所示:
* 页面中的视频处于静音状态下是允许自动播放的，大概这个产品交互可以参考 微博的主站策略

* 如果用户与当前页面有任何的交互比如点击或者 tap 这样的行为，视频的自动播放将会允许(Safari 最早采用这个策略)

* 在移动端如果页面是被添加到桌面上，自动播放是云讯的。

* 在桌面端我们会根据用户的 媒体参与指数Media Engagement Index, MEI，后文会详细说这个) 来决定这个视频是否自动播放。

如何判断当前媒体是否允许自动播放可以参考下面代码:
### iframe 下自动播放的调整
### 媒体参与指数 (Media Engagement Index)
### 移动web如何实现自动播放的支持

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/hongqx/hongqx.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
