![Windrecorder](https://github.com/yuka-friends/Windrecorder/blob/main/__assets__/product-header-cn.jpg)
<h1 align="center"> 🦝 Windrecorder | 捕风记录仪</h1>
<p align="center"> An Open Source <a href="https://www.rewind.ai/">Rewind</a>'s alternative tool on Windows to help you retrieve memory cues.</p>
<p align="center">一款运行在 Windows 平台上的 <a href="https://www.rewind.ai/">Rewind</a> 替代工具，帮助你找回记忆线索</p>

<p align="center"> <a href="https://github.com/yuka-friends/Windrecorder/blob/main/README.md">English</a>  | <a href="https://github.com/yuka-friends/Windrecorder/blob/main/__assets__/README-sc.md">简体中文</a></p>

---

捕风记录仪 是一款通过记录屏幕上所有内容、从而实现记忆搜索的应用。它可以根据 OCR 文本、或对画面的描述进行搜索，摘要浏览活动。它的所有能力都完全运行在本地，无需联网或上传任何数据，你完全拥有所有数据。

![Windrecorder](https://github.com/yuka-friends/Windrecorder/blob/main/__assets__/product-preview-cn.jpg)

**Windrecorder 目前可以做到：**
- 以较小的文件体积稳定持续地录制多个或单个屏幕；
- 只索引发生变化的画面，记录其 OCR 文本、页面标题等信息到数据库；在无人使用电脑时，自动维护数据库、清理、压缩视频；
- 完善的 webui 界面，可以回溯画面、进行 OCR /图像语义等查询；
- 提供活动统计、词云、时间轴、光箱、散点图等数据摘要；
- 支持多语言。目前内建有：简体中文、English、日本語。Welcome to contribute multilingual translations and help us improve copywriting quality.
- _coming soon... 请关注我们的 PR_

**Windrecorder 目前局限：**
- FFmpeg 在部分情况下可能会有较大内存占用；

---

> [!WARNING]
> 该项目仍在较早期开发阶段，体验与使用上可能会遇上些小问题，欢迎提出 issue 反馈、关注更新、在 [Discussions 讨论区](https://github.com/yuka-friends/Windrecorder/discussions)发起讨论与查看 roadmap。也欢迎帮助我们优化与构建项目，提出 PR / review。

> [!IMPORTANT]  
> 由于代码编写小失误，`0.0.5` 以前版本可能无法正常检测更新、或通过 install_update.bat 进行升级。如是，请在 `Windrecorder` 根目录的路径框输入`cmd`打开命令行，输入`git pull`进行更新。🙇‍♀️

# 🦝 安装

- 下载 [ffmpeg](https://github.com/BtbN/FFmpeg-Builds/releases)（下载文件名为：`ffmpeg-master-latest-win64-gpl-shared.zip`） ，将 bin 目录下的所有文件复制至 `C:\Windows\System32` 下（或其他位于 PATH 的目录下）（不包括 bin 目录本身）
    - ffmpeg 可能有“在录制屏幕时光标会闪烁”的 bug，可以先根据底下 Q&A 进行修复后、再拷贝至系统目录；

- 安装 [Git](https://git-scm.com/download/win)，一路下一步即可；

- 安装 [Python](https://www.python.org/ftp/python/3.11.7/python-3.11.7-amd64.exe)，安装时确保勾选 `Add python.exe to PATH`
    - **注意！目前暂未支持 python 3.12**，推荐使用 python 3.11，即上面链接指向的版本

- 在文件管理器中，导航到想要安装此工具目录（推荐放在空间富足的分区中），通过终端命令 `git clone https://github.com/yuka-friends/Windrecorder` 下载该工具；

    - 可以打开想要安装的文件夹，在路径栏输入`cmd` 并回车，即可在终端定位到当前目录，将以上命令贴入、回车执行；

    - 如果目录路径中包含空格，启动 app 时可能会失败；[#110](https://github.com/yuka-friends/Windrecorder/issues/110)

- 打开目录下的 `install_update.bat` 进行工具安装与配置，顺利的话就可以开始使用了！


# 🦝 如何使用

- 打开目录下的 `start_app.bat`，工具会运行在系统托盘，通过右键菜单使用；
- 所有的数据（视频、数据库、统计信息）将会存储于 Windrecorder 同目录下。如想拷贝、移动工具位置（比如更换了电脑），只需删除目录下 `.venv`、在移动文件夹后，重新运行 `install_update.bat` 安装虚拟环境即可使用；

> [!TIP]
> 最佳实践：在 webui 中设置开机自启动，即可无感记录下一切。
> 
> **当画面没有变化、或屏幕睡眠时将自动暂停记录。当电脑空闲无人使用时，工具会自动维护数据库、压缩、清理过期视频。**
> 
> _Just set it and forget it！_


# 🦝 运作原理
![Windrecorder](https://github.com/yuka-friends/Windrecorder/blob/main/__assets__/how-it-work-sc.jpg)

当启动记录后，捕风记录仪将逐段录制 15 分钟的视频，在录制完毕后对视频片段进行索引（因此，数据的查询可能会有 15 分钟的延迟时间）。当屏幕没有变化、窗口标题在跳过列表、或电脑进入锁屏时，将会自动暂停录制，并进行闲时维护（压缩与清理视频、进行图像嵌入识别等），直到用户回来、继续操作电脑。

- _图像嵌入索引以扩展形式提供，可以在目录 `extension/install_img_embedding_module` 下进行安装_

| 视频录制大小                                                                            | SQlite 数据库大小           |
|-----------------------------------------------------------------------------------------|---------------------------|
| 每小时：2-100 Mb (取决于画面变化\显示器数量)                                            |                              |
| 每个月：10-20 Gb (取决于屏幕时间) 不同的视频压缩预设，可将这些数据压缩至 0.1-0.7 倍大小    | 每个月：约 160 Mb             |

> 未来可能会改进录制方法，降低 ffmpeg 资源占用、让回溯不必等待。目前 ffmpeg 在录制时可能有较高的内存占用。


# 🦝 Q&A | 常见问题

Q: 录制过程中鼠标闪烁

- A：FFmpeg 历史遗留问题，可尝试[该帖](https://stackoverflow.com/questions/34023630/how-to-avoid-mouse-pointer-flicker-when-capture-a-window-by-ffmpeg)方法解决：
    - 使用任意十六进制编辑器（如 [HxD](https://mh-nexus.de/en/downloads.php?product=HxD20)）打开之前下载的 `FFmpeg/bin` 中的 `avdevice-XX.dll` 文件；
    - 搜索 hex code（字节序列） `20 00 cc 40`，将其最后两位 `40` 改为 `00`；
    - 保存文件即可；


Q: 打开 webui 时没有近期一段时间的数据。

- A: 当工具正在索引数据时，webui 将不会创建最新的临时数据库文件。
解决方法：尝试稍等一段时间，等待工具索引完毕后，刷新 webui 界面，或删除 db 目录下后缀为 _TEMP_READ.db 的数据库文件后刷新即可（若出现数据库文件损坏提示，不必担心，可能是工具仍然在索引中，请尝试过段时间刷新/删除）。此项策略未来将会修复重构。 [#26](https://github.com/yuka-friends/Windrecorder/issues/26)

Q: 在打开webui时提示：`FileNotFoundError: [WinError 2] The system cannot find the file specified: './db\\user_2023-10_wind.db-journal'`

- A: 通常在初次访问 webui 时、工具仍正在索引数据时出现。
解决方法：在工具后台索引完毕后，删除 db 文件夹下对应后缀为 _TEMP_READ.db 的数据库文件后刷新即可。

Q: Windows.Media.Ocr.Cli OCR 不可用/识别率过低

- A1: 检查系统中是否添加了目标语言的语言包/输入法：https://learn.microsoft.com/en-us/uwp/api/windows.media.ocr

- A2: Windows.Media.Ocr.Cli 对较小的文本识别率可能不良，通过在设置中打开「相近字形搜索」选项可以提高搜索时的召回命中率。未来将会添加对更多本地 OCR 工具的支持。

# 🧡
引入了这些项目的帮助：

- https://github.com/DayBreak-u/chineseocr_lite
- https://github.com/zh-h/Windows.Media.Ocr.Cli
- https://github.com/unum-cloud/uform
- https://github.com/streamlit/streamlit


---

🧡 喜欢这个工具？欢迎到 Youtube 与流媒体音乐平台上听听 [長瀬有花 / YUKA NAGASE](https://www.youtube.com/channel/UCf-PcSHzYAtfcoiBr5C9DZA) 温柔的音乐，谢谢！

> "Your tools suck, check out my girl Yuka Nagase, she's amazing, I code 10 times faster when listening to her." -- @jpswing
---
在 Product Hunt 上为 **捕风记录仪** 投票:

<a href="https://www.producthunt.com/posts/windrecorder?utm_source=badge-featured&utm_medium=badge&utm_souce=badge-windrecorder" target="_blank"><img src="https://api.producthunt.com/widgets/embed-image/v1/featured.svg?post_id=441411&theme=neutral" alt="Windrecorder - search&#0032;&#0038;&#0032;rewind&#0032;everything&#0032;happened&#0032;on&#0032;your&#0032;screen | Product Hunt" style="width: 250px; height: 54px;" width="250" height="54" /></a>

---
> Also checkout:
> - [🧡 after-you](https://github.com/yuka-friends/after-you): a local-first AI diary app, responding to your heart's call