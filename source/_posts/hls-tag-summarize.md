---
title: hls-tag-summarize
date: 2018-10-02 11:36:14
tags: 
- hls
- h5player
- m3u8
categories:
- fe
toc: true
---
自己接触hls很久了，自从开始接触h5播放器，就开始接触hls、m3u8这些关键词，对这些概念和使用方式都能详细说出，感觉了解还挺多，但是上周遇到一个公司的视频在ios系统的UC浏览器上总是只播放广告之后，中断了，无法继续播放的问题，排查结果是UC自己的播放器遇到EXT-X-TARGETDURATION自动结束播放，而公司内的针对广告和正片的playlist合并，每个广告内容结束都会有EXT-X-TARGETDURATION，于是UC的开发和我开始了关于这个标签的撕逼，为了不给打脸，看来得对hls的各种tag有个总结了![](/assets/emoji/wulian.gif)
<!-- more -->

### 关于hls
   **HLS**，即**HTTP Live Streaming**，是一个由苹果公司提出的基于HTTP的流媒体网络传输协议, 其工作原理是把整个流分成有一个个小的基于http的文件来做传输，当媒体流正在播放时，客户端可以选择从许多不同的备用源中以不同的速率下载同样的资源，允许流媒体会话适应不同的数据速率。客户端会先下载一个包含元数据列表的[extended M3U (m3u8)playlist](https://zh.wikipedia.org/w/index.php?title=Extended_M3U&action=edit&redlink=1)文件，按照一定的规则解析文件内容，从而得到可用的媒体流地址，再持续下载媒体资源。

### 协议简介
   > * 视频单个媒资资源的封装格式必须是ts
   > * 视频的编码格式为H264,音频编码格式为MP3、AAC或者AC-3
   > * 必须要有用于控制的m3u8索引列表

### 使用范围
   > 1. 因为该协议由苹果推出，所以在苹果上的支持也是最好的，在iphone或者是mac上，可以直接将m3u8文件作为videoElement的src，支持原生播放

   > 2. 在chrome下或者android系统下是无法播放的(除非浏览器支持该视频类型的解码)

   > 3. 用于web端直播，自从adobe宣布不再对flash做维护开始，web端的直播支持成了一个继续解决的问题，而hls支持不断追加m3u8文件中的ts列表实现直播流的传输（当然有一个弊端就是直播会有延时,延迟时间无法下到10秒以下）,目前web端的直播大多数都通过hls来实现

   > 4. 用于web端的点播。web端点播方案主要依赖于html5标准中的videoElement/audioElement, 支持的最普遍的编码格式为mp4, 但是使用videoElement播放视频有诸多弊端，主要来自缓存和分片两个方面，随着**MSE**([Media Source Extensions API](https://developer.mozilla.org/zh-CN/docs/Web/API/Media_Source_Extensions_API))这个API的出现，可以结合HLS格式进行视频流的加载，目前比较流行的实现是**[hls.js](https://github.com/video-dev/hls.js)**这个库

### 关于m3u8
m3u8即是hls协议的index文件，一级index文件可以指定不同码率的二级m3u8文件的列表如示例1中所示，二级的index文件负责输出ts文件的下载地址，如示例2中所示

**m3u8示例1**

```code
#EXTM3U
#EXT-X-STREAM-INF:BANDWIDTH=150000,RESOLUTION=416x234,CODECS="avc1.42e00a,mp4a.40.2"
http://example.com/low/index.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=240000,RESOLUTION=416x234,CODECS="avc1.42e00a,mp4a.40.2"
http://example.com/lo_mid/index.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=440000,RESOLUTION=416x234,CODECS="avc1.42e00a,mp4a.40.2"
http://example.com/hi_mid/index.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=640000,RESOLUTION=640x360,CODECS="avc1.42e00a,mp4a.40.2"
http://example.com/high/index.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=64000,CODECS="mp4a.40.5"
http://example.com/audio/index.m3u8
```

**m3u8示例2**
```code
#EXTM3U
#EXT-X-TARGETDURATION:10
#EXT-X-VERSION:3
#EXTINF:10,
000.ts
#EXTINF:10,
001.ts
#EXTINF:10,
002.ts
#EXT-X-DISCONTINUITY
#EXTINF:10,
010.ts
#EXTINF:10,
010.ts
#EXT-X-ENDLIST
```
#### 关于常见格式.m3u和.m3u8
 M3U文件中，以.m3u8结尾，或其http头中”Content-Type”字段为” application/vnd.apple.mpegurl”会以UTF-8编码，而以”m3u” 或其http对应字段为” "audio/mpegurl”会以US-ASCII编码。播放列表文请求头类型必须以上面某一匹配对对应
[该说明参考文档](https://blog.csdn.net/zhoushuaiyin/article/details/38759427?utm_source=copy)
#### tags
##### 一级索引tag
> * 
##### 二级索引tag
##### EXT-X-DISCONTINUITY和EXT-X-ENDLIST的区别
EXT-X-DISCONTINUITY和#EXT-X-ENDLIST这两个标签，都有中断的意思，这也是之前我和UC的播放器的开发出现分歧的地方，UC劫持video之后的播放器，在检测到EXT-X-DISCONTINUITY这个标签的时候，就直接中断视频的加载，认为播放结束了,我司播放视频会将广告和正片合并成为一个文件，合并的时候广告ts列表和正片ts列表中间会出现EXT-X-DISCONTINUITY标签，在UC浏览器中只能播放广告，然后就停住了，停住了，停住了!!! ![](/assets/emoji/icons-50_02.png)

> * **EXT-X-DISCONTINUITY**表示当前分割片段和紧随其后的片段之间的编码是间断的，一般出现这种情况是因为文件格式、媒体轨道的数量和类型、编码参数、编码序列、时间戳序列等发生了变化，需要在m3u8中加入#EXT-X-DISCONTINUITY隔开，让播放器解码流程重新初始化，这个标签在分片列表中可以出现多次。一般在业务处理中会用于不同媒体资源的拼接连播或者是广告片段的插入
[参考规范3.4.11](https://tools.ietf.org/html/draft-pantos-http-live-streaming-13#section-3.4.11)

> * **EXT-X-ENDLIST**表示在当前媒体ts列表中已经没有分片了，这个标签可以在列表的任何地方出现，但是在playlist中只能出现一次，一般点播按理中m3u8文件该标签都会出现在列表最后一行，标识播放完成；而在直播文件中添加了该标签标识直播结束，后面追加的ts不会再被加载。
[参考规范3.4.8](https://tools.ietf.org/html/draft-pantos-http-live-streaming-13#section-3.4.8)

根据以上对两个标签的说明，UC播放器的处理方式显然是有问题的，但是对方也提出一个就是加了EXT-X-DISCONTINUITY的文件，在ffmpeg播放器中也无法播放，我查了下，ffmpeg播放器不支持EXT-X-DISCONTINUITY[说明链接](https://trac.ffmpeg.org/ticket/5419)，但是从规范的说明和实际的业务场景（主要是广告![](/assets/emoji/wulian.gif)),EXT-X-DISCONTINUITY是个必须支持的标签。。。当然针对这个标签，也会有一些兼容性的问题，例如我遇到的无法加载，或者该标签后面的内容没有画面等等，也是个坑，不过apple的原生处理（safari）下是支持的，播放并不会出现问题。
