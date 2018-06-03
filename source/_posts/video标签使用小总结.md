---
title: video标签使用小总结
date: 2017-11-21 09:18:45
tags: 
---

- 属性
- 方法
- 事件

### 1. 示例：

```
<video src="movie.ogg" controls="controls">
    您的浏览器不支持 video 标签。
</video>
```

```
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
```
ps：<track> 标签用于规定字幕文件或其他包含文本的文件，当媒介播放时，这些文件是可见的。不过主流浏览器基本不支持所以不总结。

### 2. 标签属性

| 属性 | 说明 |
|:-:|:-:|
| autoplay  | 视频加载完马上播放 |
| controls | 显示控件（用户可操作）|
| loop | 重复播放 |
| muted | 视频的音频输出为静音|
| poster | 规定视频正在下载时显示的图像，直到用户点击播放按钮 |
| preload | none:不预载， metadata:预载资源信息， auto: |
| src| 视频的URL |
| height | 视频播放器的高度 |
| width | 视频播放器的宽度 |

### 2.1 video对象属性

```
var Media = document.getElementById("myVideo");
```

| 属性 | 说明 | 示例| 注意 |
|---|---|---|--- |
| src | 设置或返回视频的 src 属性值。| Media.src=src | -|
| currentSrc | 获得当前视频的 URL| Media.currentSrc| (只读) |
| currentTime | 当前播放时间，单位为秒。为其赋值将会使媒体跳到一个新的时间。 | Media.currentTime | -|
| duration | 返回当前视频的长度 | Media.duration|(只读)不同的浏览器返回不同的值 |
| buffered | 返回 TimeRanges 对象(表示用户的音视频缓冲范围)。|Media.buffered.start(0)/Media.buffered.end(0) | 缓冲范围指的是已缓冲音视频的时间范围。如果用户在音视频中跳跃播放，会得到多个缓冲范围 |
| ended |返回视频是否已结束| Media.ended | 返回true/false |
| error| 获得视频的错误状态 | Media.error.code | (只读)1.用户终止 2.网络错误 3.解码错误 4.URL无效 |
| paused | 返回视频是否已暂停 | Media.paused | (只读)返回true/false |
| seeking | 属性返回用户目前是否在音频/视频中寻址。寻址中（Seeking）指的是用户在音频/视频中移动/跳跃到新的位置。| - | (只读)返回true/false|
| volume | 设置或返回视频的当前音量 |Media.volume/Media.volume=number | number必须是介于 0.0 与 1.0 之间的数字。1默认（最高音量）|
| networkState | 返回音频/视频的当前网络状态 | Media.networkState | 0.此元素未初始化 1.正常但没有使用网络 2.正在下载数据 3.没有找到资源 |
| defaultPlaybackRate | 设置或返回音频的默认播放速度 |Media.defaultPlaybackRate/Media.defaultPlaybackRate=number | 正常速度：1.0；设置该属性仅会改变默认的播放速度，而不是当前的。|
| playbackRate | 设置或返回视频的当前播放速度。|Media.playbackRate/Media.playbackRate=playbackspeed | 默认速度1.0 |
| readyState | 返回视频的当前就绪状态。 | Media.readyState | 0-没有关于音频/视频是否就绪的信息;1-关于音频/视频就绪的元数据;2-关于当前播放位置的数据是可用的，但没有足够的数据来播放下一帧/毫秒;3当前及至少下一帧的数据是可用的；4- 可用数据足以开始播放 |


ps：有些属性支持性不是很好就不总结，例如：defaultMuted 

### 3. 方法

| 方法 | 说明 | 示例 |
|---|---|---|
| play() | 播放 | Media.play() |
| pause() | 暂停 | Media.pause() |
| load() | 重新加载音频/视频元素(在更改来源或其他设置后对音频/视频元素进行更新) | Media.src = "movie.ogg"; Media.load(); |
| canPlayType() | 是否能播放某种格式的资源 | 返回值："probably" - 最有可能支持；"maybe" - 可能支持；"" - （空字符串）不支持 |



### 4. 事件

（总结几个比较常用的事件）

语法举例（其他类似）：
```
<video onplay="myScript">

video.onplay=function(){myScript};

video.addEventListener("play", myScript);
```


事件 | 说明
---|---
play  | 在音频/视频(audio/video)开始播放时触发。
playing | 在音频/视频(audio/video)因缓冲而暂停或停止后已就绪时触发。
pause  | 在音频/视频(audio/video)暂停时触发。
ended|在音频/视频(audio/video)播放完成后触发。
waiting|在视频由于需要缓冲下一帧而停止时触发。
timeupdate |音频/视频（audio/video）的播放位置发生改变时触发。（移动视频播放位置可调用）
error|在音频/视频(audio/video)加载发生错误时触发。

> 当音频/视频处于加载过程中时，会依次发生以下事件：

| 事件 | 说明 |
|---|---|
| loadstart | 当加载过程开始时 |
| durationchange | 当指定音频/视频的时长数据发生变化时，会发生durationchange 事件。|
| loadedmetadata | 当指定的音频/视频的元数据（时长、尺寸（仅视频）以及文本轨道）已加载时，会发生 loadedmetadata 事件。|
| loadeddata | 当当前帧的数据已加载，但没有足够的数据来播放指定音频/视频的下一帧时，会发生 loadeddata 事件。 |
| progress | 当浏览器正在下载指定的音频/视频时，会发生 progress 事件。|
| canplay | 当浏览器能够开始播放指定的音频/视频时 |
| canplaythrough | 当浏览器预计能够在不停下来进行缓冲的情况下持续播放指定的音频/视频时，会发生 canplaythrough 事件。|

走过路过别错过，批评好评都接受，哈哈哈哈哈哈哈~~





