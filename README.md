## 介绍
AVFoundation是非常强大的音视频处理框架。例如，可以使用它来检查，创建，编辑或重新编码媒体文件。您还可以在实时捕获和播放期间从设备获取输入流并操纵视频。下图为AVFoundation在iOS上的结构体系。

![图1-1](图1-1.png)


## 播放视频

iOS上的视频播放方式：

* AVPlayer (可自定义UI界面)
* MPMoviePlayerViewController
* AVPlayerViewController(AVKit,iOS9*)
* IJKPlayer等第三方框架（基于ffmpeg，支持多种视频格式）

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

### AVPlayerViewController

在iOS 9中引入了画中画（PiP）播放功能。它可让iPad用户在可移动，可调整大小的窗口中播放视频，从而在屏幕上浮动。它为iPad带来了新的多任务功能，使用户能够在其设备上执行其他活动时继续播放。该功能可以在Apple的内置视频播放应用程序中找到，并可通过AVKit框架向您的应用程序提供。更多画中画配置参考[官方资料](https://developer.apple.com/library/content/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/UsingAVKitPlatformFeatures/UsingAVKitPlatformFeatures.html#//apple_ref/doc/uid/TP40016757-CH5-SW3)。

```
let playVC = AVPlayerViewController()

let url = URL(string: "http://static.tripbe.com/videofiles/20121214/9533522808.f4v.mp4")!
let item = AVPlayerItem(url: url)
    
playVC.player = AVPlayer(playerItem: item)
playVC.player?.play()
    
present(playVC, animated: true, completion: nil)
```

## 视频录制

视频录制的方式：

* UIImagePickerController
* AVFoundation+AVCaptureMovieFileOutput
* AVFoundation+AVAssetWriter

### UIImagePickerController
UIImagePickerController通过自定义UI来控制开始、结束录制视频。

```
//使用自定义view
cameraOverlayView
showsCameraControls = false

//开始录制
startVideoCapture()

//结束录制
stopVideoCapture()

//在代理方法中获取录制视频信息
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any])

```

### AVFoundation录制视频

使用AVCaptureMovieFileOutput来进行视频的录制不能暂停视频的录制。这里主要介绍