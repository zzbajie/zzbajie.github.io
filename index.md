《Ubuntu20.04.1使用记录》

工欲善其事,必先利其器。

近期将笔记本操作系统从ubuntu18.04升级到了Ubuntu20.04.1，其实系统早就提示升级，一直没有时间升级，笔者不喜欢直接升级系统，而是每次都安装纯净系统，反正安装时可以自定义分区挂载我的home目录，我使用的软件都是安装在docker容器，即使是微信、QQ迅雷等win下常用软件，现在部分也都支持docker了，虽然不完美但是很实用，现将系统升级流程简单说一下，以及安装中要注意的地方、和一些必备软件。

一、创建一个启动盘，笔者使用的是U盘启动，Thinkpad开机按F12（视情况），总之选择从U盘引导进入，按提示操作即可。

1、使用启动盘创建器。
  
  参考：https://help.ubuntu.com/stable/ubuntu-help/addremove-creator.html

2、安装系统很顺利，第一个要注意的是安装时选择覆盖安装，还是升级安装，还是自定义磁盘分区安装，我选择的是自定义磁盘安装 + 最小化安装(因为不需要安装一些没用的，比如纸牌啦，自带那个office啦，等等)，这样系统安装好就比较纯净。

  以下视您的情况而定，如果是uefi启动的可以参考：
  
  首先要确定系统要安装在哪个盘的哪个分区，一定不要搞错（提前备份重要数据，笔者专门用一个1T的硬盘平时也是只用于备份同步，推荐安装FreeFileSync软件，平时备份直接镜像同步备份,就不担心系统丢失了），uefi分区(我分了100M,实际只用了几兆，反正也不差那点)、swap缓存分区（与内存大小一致）。
  
  另外一个就是挂载home目录至某个分区，顺便一提，我使用的是双硬盘，一个ssd固态128G作为系统盘绰绰有余，另一个机械硬盘会挂载Home目录，另外建议把原系统HOME目录下的配置文件(如.config .cache等等安装软件时自动生成的目录)全部迁移走，或者删除，要不然新系统会与原系统的配置文件混在一起就不纯净了，说不定某些软件就冲突了，注意配置文件也要备份，洁癖就洁癖吧，可以体验到新系统的原生配置。当然以上并不是全部，具体视情况而定。
  
  还要注意一点，安装时取消勾选 “为图形或无线硬件吧啦的” 这一项，一定不要选，否则国内这个环境安装慢的让你等的黄瓜菜都凉了。
  
  安装好以后要做的工作：
  
  #更新系统及软件包================================================================
  打开软件》更新》根据提示更新操作系统内核。
  apt update 更新系统
  apt upgrate 更新软件包
  打开软件更新》附加驱动》只更新显卡驱动(重启电脑生效)

  #安装VIM=========================================================================
  sudo apt -y install vim
  
  #安装五笔输入法=================================================================
  sudo apt install fcitx-table-wbpy
  im-config 确定/是
  #选择fcitx即可
  #重启生效,重启后,取消掉》简繁切换的快捷键：ctrl+shift+f fcitx配置》附加组件》简繁切换》配置》切换来禁用启用》按ESC设置为空
  
  #安装docker(笔者是重度docker患者，自从用了这货，很少折腾操作系统了，哪个软件或配置环境不好用，直接删除，不带走一片云彩)=====================================================
  官方安装文档 https://docs.docker.com/engine/install/ubuntu/
  #安装好以后，把当前用户加入到docker用户组，可免sudo
  sudo gpasswd -a ${USER} docker
  sudo service docker restart
  newgrp docker
  #因为 /var/run/docker.sock 所属 docker 组具有 setuid 权限,还需要执行 sudo chmod a+rw /var/run/docker.sock
  sudo ls -l /var/run/docker.sock
  默认权限#srw-rw---- 1 root docker 0 May  1 21:35 /var/run/docker.sock
  执行完后#srw-rw-rw- 1 root docker 0 10月  4 16:07 /var/run/docker.sock
  docker-composer官方安装文档 https://docs.docker.com/compose/install/
  
  #安装chrome=====================================================================
  https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
  确保已科学上网，设置系统代理127.0.0.1:8118 (根据情况)，双击.deb包，使用系统自带软件管理器，进行安装。
  取消打开chrome提示输入密码：打开“应用程序》密码和密钥”》右键左侧菜单“登录”，修改密码，输入原密码（操作系统密码），新密码留空，确认即可。
  
  #安装gnome-tweak-tool===========================================================
  sudo add-apt-repository universe #这一步可选，因为ubuntu 20.04默认已经设置过
  sudo apt install gnome-tweak-tool

  https://extensions.gnome.org/extension/307/dash-to-dock/
  https://extensions.gnome.org/extension/1194/show-desktop-button/

  sudo apt install dconf-editor
  终端输入命令：dconf-editor
  调整所有窗口最小化与最大化位置路径(关闭、最大化、最小化)：/org/gnome/desktop/wm/preferences/button-layout
  
  #安装tilda（不要太好用）===========================================================
  sudo apt -y install tilda

  #安装wps=======================================================================
  https://linux.wps.cn/

  #安装filezilla=================================================================
  sudo apt -y install filezilla

  #安装freefilesync===========================================================
  https://freefilesync.org/
  
  #安装vscode
  https://code.visualstudio.com/
  将界面转换为中文：使用快捷键【Ctrl+Shift+P】在弹出的搜索框中输入【configure language】，然后选择搜索出来的【Configure Display Language】,选择左侧安装简体中文即可。
  
  #安装vmware=================================================================
  Serial keys:
  ZF3R0-FHED2-M80TY-8QYGC-NPKYF
  YF390-0HF8P-M81RQ-2DXQE-M2UT6
  ZF71R-DMX85-08DQY-8YMNC-PPHV8
  AZ3E8-DCD8J-0842Z-N6NZE-XPKYF
  FC11K-00DE0-0800Z-04Z5E-MC8T6(已测,可用)
  If the keys worked for you, this(https://isha.sadhguru.org/mahashivratri/sadhguru/articles/shambho-a-gentle-form-of-shiva/) will also work.
  取消3D后，可安装vmware Tool，否则报错显卡问题，会导致安装失败。
  
  #安装截图工具（不要太好用）==============================================================
  sudo apt install flameshot
  设置一下快捷键
  进入系统设置中的“键盘设置” 页面中会列出所有现有的键盘快捷键，拉到底部就会看见一个 “+” 按钮 点击 “+” 按钮添加自定义快捷键并输入以下两个字段：
  名称： 任意名称均可。
  命令： flameshot gui
  快捷键：根据自己习惯设定
  
  #安装 启动盘创建器(ubuntu20不带这货，因为我选择的是最小化安装)
  sudo apt update
  sudo apt install usb-creator-gtk

  #将地球的近实时图片设置为桌面背景（利用日本国家的一颗近地卫星）========================================
  https://github.com/boramalper/himawaripy
  which -- himawaripy
  crontab -e
  #首次执行crontab，会提示选择一个默认的编辑器，此时选择3. /usr/bin/vim.basic
  */10 * * * * /usr/local/bin/himawaripy > /dev/null

3、安装微信、QQ、迅雷 这几个国内扛把子，有时候还真离不开。
  docker说了，要统治操作系统的整个世界~真正的用完就走，比如现在docker里就能跑浏览器，跑win下的软件，利于主机的gui，当然了目前全部都实现不行。docker啥时候说了～。
  linux下使用微信很简单，而且捣鼓出来还带声音，视频还没搞定。
  不多说了，上命令：
  #安装微信#################################################
  参考的这位大神的：https://github.com/bestwu/docker-wechat
  #docker-compose.yml
  version: "3"
  services:
  wechat:
    image: bestwu/wechat
    container_name: wechat
    #privileged: true
    devices:
      - /dev/snd
      #- /dev/video0 #然并卵
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - $HOME/桌面/WeChatFiles:/WeChatFiles
      - ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native
      - $HOME/.config/pulse/cookie:/root/.config/pulse/cookie #我不知道为什么需要cookie。没有cookie，就会发生错误ALSA lib pulse.c:243:(pulse_connect) PulseAudio: Unable to connect: Access denied
    environment:
      - DISPLAY=unix$DISPLAY
      - QT_IM_MODULE=fcitx
      - XMODIFIERS=@im=fcitx
      - GTK_IM_MODULE=fcitx
      ###解决声音设备访问权限问题
      # 设置PULSE_SERVER以便让容器的pulseaudio知道服务器地址。
      # apt update && apt install alsa-base alsa-utils pulseaudio
      - PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native
      # 添加容器到该audio组，查看：cat /etc/group|grep audio
      - AUDIO_GID=29
      #- VIDEO_GID=44 # 可选(然并卵) 查看：cat /etc/group | grep video 主机video gid 解决视频设备访问权限问题
      - GID=1000 # 可选 默认1000 主机当前用户 gid 解决挂载目录访问权限问题
      - UID=1000 # 可选 默认1000 主机当前用户 uid 解决挂载目录访问权限问题
    ipc: host


  #安装QQ#############################################
  https://github.com/bestwu/docker-qq
  version: '3'
  services:
  qq:
    image: bestwu/qq
    container_name: qq
    #privileged: true
    devices:
      - /dev/snd #声音
      #- /dev/video0 #然并卵
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - $HOME/桌面/TencentFiles:/TencentFiles
      - ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native
      - $HOME/.config/pulse/cookie:/root/.config/pulse/cookie #我不知道为什么需要cookie。没有cookie，就会发生错误ALSA lib pulse.c:243:(pulse_connect) PulseAudio: Unable to connect: Access denied
    environment:
      - DISPLAY=unix$DISPLAY
      - QT_IM_MODULE=fcitx
      - XMODIFIERS=@im=fcitx
      - GTK_IM_MODULE=fcitx
      ###解决声音设备访问权限问题
      # 设置PULSE_SERVER以便让容器的pulseaudio知道服务器地址。
      # apt update && apt install alsa-base alsa-utils pulseaudio
      - PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native
      # 添加容器到该audio组，查看：cat /etc/group|grep audio
      - AUDIO_GID=29
      #- VIDEO_GID=44 # 可选(然并卵) 查看：cat /etc/group | grep video 主机video gid 解决视频设备访问权限问题
      - GID=1000 # 可选 默认1000 主机当前用户 gid 解决挂载目录访问权限问题
      - UID=1000 # 可选 默认1000 主机当前用户 uid 解决挂载目录访问权限问题
    ipc: host
  
  #安装迅雷#############################################
  version: "3"
  services:
  thunderspeed:
    image: bestwu/thunderspeed
    container_name: thunderspeed
    devices:
      - /dev/snd
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - $HOME/桌面/迅雷下载:/迅雷下载
      - ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native
      - $HOME/.config/pulse/cookie:/root/.config/pulse/cookie #我不知道为什么需要cookie。没有cookie，就会发生错误ALSA lib pulse.c:243:(pulse_connect) PulseAudio: Unable to connect: Access denied
    environment:
      - DISPLAY=unix$DISPLAY
      - QT_IM_MODULE=fcitx
      - XMODIFIERS=@im=fcitx
      - GTK_IM_MODULE=fcitx
      ###解决声音设备访问权限问题
      # 设置PULSE_SERVER以便让容器的pulseaudio知道服务器地址。
      # apt update && apt install alsa-base alsa-utils pulseaudio
      - PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native
      # 添加容器到该audio组，查看：cat /etc/group|grep audio
      - AUDIO_GID=29
      #- VIDEO_GID=44 # 可选(然并卵) 查看：cat /etc/group | grep video 主机video gid 解决视频设备访问权限问题
      - GID=1000 # 可选 默认1000 主机当前用户 gid 解决挂载目录访问权限问题
      - UID=1000 # 可选 默认1000 主机当前用户 uid 解决挂载目录访问权限问题
    ipc: host

人脸识别使用dlib的最先进的人脸识别功能和深度学习功能构建。该模型在Wild基准中的Labeled Faces上的准确性为99.38％
