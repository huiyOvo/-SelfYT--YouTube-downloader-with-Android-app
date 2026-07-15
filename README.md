SelfYT

自部署 YouTube 视频下载器，PC 做服务端，手机装 APP，走局域网，不经过任何第三方网站。


一、功能

粘贴 YouTube 链接 → 解析视频信息（标题、封面、时长）
可选视频分辨率（4K 到 144p）或 MP3 音频
实时进度条 + 速度显示
下载完成自动传到手机，支持取消和删除任务


二、两个版本

大众版
  日常下载用，视频和 MP3 都能下

专用版
  多了 NVENC 压缩，720p 压到 95MB 以内
  QQ 群文件限制 100MB 的场景用这个

一般人装大众版就行。


三、技术栈 & 依赖

PC 端（server/）

  Python 3.11+          运行环境
  Flask 3.x             Web API
  flask-cors            跨域
  yt-dlp                YouTube 解析和下载
  ffmpeg                视频合并、转码、NVENC 压缩

  安装：pip install flask flask-cors yt-dlp
  ffmpeg 需要单独装系统里，PATH 能找到就行

Android 端（app/）

  Kotlin 1.9 + Compose  UI
  Retrofit 2 + OkHttp 4  网络请求
  Gson                   JSON
  Coil                   封面图加载
  DataStore              配置持久化
  DocumentFile (SAF)     自定义目录


四、怎么用

1. PC 端
   进 server/ 目录双击 启动服务器.bat
   窗口会显示本机 IP，记下来

2. 手机端
   装 APK，和 PC 连同一个 WiFi
   打开 APP，设置里填 http://你的IP:5000
   粘贴 YouTube 链接 → 解析 → 选画质 → 下载

默认端口 5000，改端口编辑 app.py 最后一行：
  app.run(host="0.0.0.0", port=5000)

代理在 server/config.json 配：
  { "proxy": "http://127.0.0.1:7892" }
不需要就留空。


五、和命令行 yt-dlp 比

  命令行      得敲命令记参数
  SelfYT      手机点两下，不用记格式参数

  命令行      进度就是终端里的文字百分比
  SelfYT      进度条 + 实时速度 + 剩余时间

  命令行      文件下到 PC 本地
  SelfYT      自动传到手机相册

  命令行      压缩得手写 ffmpeg
  SelfYT      专用版一键 NVENC，压到 95MB


六、和第三方下载网站比

  没有广告弹窗，不会点半天跳到别的地方
  文件不过别人服务器，PC 直传手机
  不限大小、不限次数、不限速度
  自己部署，不会哪天网站打不开
  MP3 直接提，不用二次转换


七、项目结构

  SelfYT/
  ├── server/
  │   ├── app.py               Flask API
  │   ├── config.json          代理配置
  │   ├── requirements.txt     Python 依赖
  │   └── 启动服务器.bat
  ├── app/                     Android 源码
  │   └── app/src/main/java/com/ytdownloader/
  │       ├── MainActivity.kt
  │       ├── viewmodel/MainViewModel.kt
  │       ├── data/api/ApiService.kt
  │       └── ui/screen/
  │           ├── HomeScreen.kt
  │           ├── FormatScreen.kt
  │           └── DownloadScreen.kt
  ├── install_sdk.bat
  └── install_sdk.py


八、下载

  源码：GitHub 仓库
  APK：Releases 页面
