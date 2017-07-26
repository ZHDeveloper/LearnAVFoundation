## 介绍
AVFoundation是非常强大的音视频处理框架。例如，可以使用它来检查，创建，编辑或重新编码媒体文件。您还可以在实时捕获和播放期间从设备获取输入流并操纵视频。下图为AVFoundation在iOS上的结构体系。

![图1-1](图1-1.png)


## 播放视频

iOS上的视频播放方式：

* AVPlayer (可自定义UI界面)
* MPMoviePlayerViewController
* AVPlayerViewController(AVKit,iOS9*)

### AVPlayer

AVPlayer一次播放单个视频，提供了播放、暂停、播放速度等功能。AVPlayer的使用通过观察者模式（KVO）来监听播放属性、状态的变化。

AVQueuePlayer继承自AVPlayer，提供了多个媒体视频的播放功能。

在实际开发中可以根据需求选择使用，个人认为封装的AVPlayer完全可以取代AVQueuePlayer。更多实现细节参考[ZHPlayer-Swift](https://github.com/ZHDeveloper/ZHPlayer-Swift)

### MPMoviePlayerViewController
MPMoviePlayerViewController简单的全屏视频播放控制器。iOS9正式弃用，系统推荐使用AVPlayerViewController。

```
let playerVC = MPMoviePlayerViewController(contentURL: self.url)!
playerVC.moviePlayer.prepareToPlay()
playerVC.moviePlayer.play()
self.present(playerVC, animated: true, completion: nil)
```



## 2、音频

