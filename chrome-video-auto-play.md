## Chrome 新的自动播放策略
最近看 来自 Chrome 团队在 IO 2018上的 分享 [《Build awesome media experiences on the web》](https://www.youtube.com/watch?v=5azRhKsSU_M) 。大概主要是说关于 音视频在 Chrome 上的更新。其中业务团队需要及时关注新的 自动播放策略，虽然在去年9月份 Chrome 团队就更新了博客 [《Autoplay Policy Changes》](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)。 Chrome 会在 2018年 的第二个季度，采取新的自动播放策略。目前该策略在很多场景已经存在，用户初始进入到视频页面的时候，会阻止默认自动播放，如果用户在当前域名下选择了播放，在多次选择之后，就会默认可执行自动播放。目前该策略也对我们线上的一些场景产生了影响，参照之前对safari11以上版本webkit内核根据用户设置阻止视频自动播放的案例的处理，下面是针对chrome上无法自动播放的一些整理，以及在移动web端自动播放支持的情况总结。

### 自动播放的策略调整
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
