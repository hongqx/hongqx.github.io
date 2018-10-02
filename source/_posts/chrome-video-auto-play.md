---
title: web端video自动播放策略
date: 2018-07-31 09:12:42
tags: 
- videoElement
- autoplay
categories:
- fe
toc: true
---
最近看 来自 Chrome 团队在 IO 2018上的 分享 [《Build awesome media experiences on the web》](https://www.youtube.com/watch?v=5azRhKsSU_M) 。大概主要是说关于 音视频在 Chrome 上的更新。其中业务团队需要及时关注新的 自动播放策略，虽然在去年9月份 Chrome 团队就更新了博客 [《Autoplay Policy Changes》](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)。 Chrome 会在 2018年 的第二个季度，采取新的自动播放策略。

目前该策略在很多场景已经存在，用户初始进入到视频页面的时候，会阻止默认自动播放，如果用户在当前域名下选择了播放，在多次选择之后，就会默认可执行自动播放。目前该策略也对我们线上的一些场景产生了影响，参照之前对safari11以上版本webkit内核根据用户设置阻止视频自动播放的案例的处理，下面是针对chrome上无法自动播放的一些整理，以及在移动web端自动播放支持的情况总结。
<!-- more -->
### 自动播放的策略调整
![Chrome Autoplay Policy](http://img1.vued.vanthink.cn/vueda728bcacf470e6922f5ba6325af54c81.png)
如图中所示:
* 页面中的视频处于静音状态下是允许自动播放的，大概这个产品交互可以参考 微博的主站策略
* 如果用户与当前页面有任何的交互比如点击或者 tap 这样的行为，视频的自动播放将会允许(Safari 最早采用这个策略)
* 在移动端如果页面是被添加到桌面上，自动播放是云讯的。
* 在桌面端我们会根据用户的 媒体参与指数Media Engagement Index, MEI，后文会详细说这个) 来决定这个视频是否自动播放。

如何判断当前媒体是否允许自动播放可以参考下面代码:
```javascript
var promise = document.querySelector('video').play();

if (promise !== undefined) {  
    promise.catch(error => {
        // Auto-play was prevented
        // Show a UI element to let the user manually start playback
    }).then(() => {
        // Auto-play started
    });
}
```
### iframe 下自动播放的调整
Chrome 还强调了开发者可以通过 **allow** 属性来控制页面中通过 iframe 来控制引用页面内的媒体权限。
```html
<!-- Autoplay is allowed. -->  
<iframe src="https://cross-origin.com/myvideo.html" allow="autoplay">

<!-- Autoplay and Fullscreen are allowed. -->  
<iframe src="https://cross-origin.com/myvideo.html" allow="autoplay; fullscreen">  
```
*如果是同一个域下的自动播放默认是允许的*

### 媒体参与指数 (Media Engagement Index)
MEI 是一个评估用户对于当前站点的媒体参与程度的指数，它取决于下面几个维度:

用户在媒体上停留时间超过了 7秒以上
音频必须是展示出来，并且没有静音
与 video 之间有过交互
媒体的尺寸不小于 200x140.
你可以在 Chrome 的地址栏输入:
```javascript
chrome://media-engagement
```
如果你是作为开发者，你也可以手动调整这个策略:
```javascript
chrome://flags/#autoplay-policy  
```
在地址栏输入以上的地址，可以进行手动的策略调整，进行测试。
### 移动web如何实现自动播放的支持
在chrome和safari调整期自动播放的策略之前，最最让人悲桑的就是移动端的自动播放过，很多第三方浏览器是直接不允许自动播放的，例如QQ浏览器、百度浏览器，当前还有万恶的UC浏览器，针对这些浏览器，宝宝只能很悲桑的说一声，要么你的公司能强制让这些大爷们给你做可自动播放配置，要么就去挑战提出要自动播放的产品吧放弃吧~~（ps：这个自动播放从用户角度来说确实也是合理的哈）
    
以上种种，在移动web端浏览器中实现自动播放希望渺茫，但是在自家app的webview中还是有一线希望的，前提是你要去找native开发哥哥帮你修改个webview的参数啦
android需要如下修改
```javascript
mWebview.getSettings().setMediaPlaybackRequiresUserGesture(false);
```
ios需要添加webview的属性设置
```javascript
 //media的播放必须要用户行为触发
_webView.mediaPlaybackRequiresUserAction = NO;
```
如果希望自动播放时不要全屏，那么嗨需要设置下面的属性
```javascript
 //允许media在行间播放
_webView.allowsInlineMediaPlayback = YES;
```