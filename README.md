# android_webrtc_h265
谷歌的webrtc，默认不支持h265，好在网上有开源的实现，但是想要传输层支持h265需要重新编译生成libjingle_peerconnection_so.so。本篇主要记录一下编译过程。
[项目github地址:https://github.com/open-webrtc-toolkit/owt-client-native](https://github.com/open-webrtc-toolkit/owt-client-native)
自己通过虚拟机安装Ubuntu18.04并翻墙，无论如何都无法编译成功，无奈，购买了1个月香港云主机，编译成功！

假设当前路径为`/home/zhanggf`
### 1.下载代码
`git clone https://github.com/open-webrtc-toolkit/owt-client-native.git`
下载完成后，将目录重命名为src
`mv owt-client-native src`
### 2.下载depot_tools
`git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git`
下载完成后，设置PATH
`export PATH=$PATH:/home/zhanggf/depot_tools`
### 3.创建.gclient
`vi .gclient`
将以下内容复制到文件中并保存：
```
solutions = [
  {
     "managed": False,
     "name": "src",
     "url": "https://github.com/open-webrtc-toolkit/owt-client-native.git",
     "custom_deps": {},
     "deps_file": "DEPS",
     "safesync_url": "",
  },
]
target_os = ["android"]
```
### 4.gclient sync
进入src目录`cd src`,执行`gclient sync`，后面就是漫长的等待，视网速情况，需要1-n个小时吧。
### 5.编译
进入/src/scripts目录,执行`python build_android.py`开始编译。全部编译完成大概1小时左右吧。
### 6.out
最终生成的目标文件在src/out路径下，包括libwebrtc.jar和对应版本的libjingle_peerconnection_so.so。替换当前项目中的相关文件，恭喜你，你的webrtc项目已经支持h265硬编码了。