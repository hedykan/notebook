# synaptics触控板驱动

## 切换到X11后端
sudo emacs -nw /etc/gdm3/custom.conf
```conf
    WaylandEnable=false
```
## 下载驱动
下载[xserver-xorg-input-synaptics_1.9.1-ubuntu3_amd64.deb](https://launchpad.net/ubuntu/+archive/primary/+files/xserver-xorg-input-synaptics_1.9.1-1ubuntu3_amd64.deb)
或者`sudo apt install xserver-xorg-input-synaptics`

## 在xorg设置
安装完驱动后可以直接`synclient -l`展示可以设置的选项
所有设置选项在[xorg文档](https://www.x.org/releases/X11R7.6/doc/man/man4/synaptics.4.xhtml)中
可以参考[arch linux](https://wiki.archlinuxcn.org/wiki/Touchpad_Synaptics)的设置
设置配置文件地址`/etc/X11/xorg.conf.d/70-synaptics.conf`
```conf
Section "InputClass"
       Identifier "touchpad"
       Driver "synaptics"
       MatchIsTouchpad "on"
              Option "TapButton1" "1"
              Option "TapButton2" "3"
              Option "TapButton3" "2"
              Option "VertScrollDelta" "-111"
              Option "HorizScrollDelta" "-111"
              Option "VertTwoFingerScroll" "on"
              Option "HorizTwoFingerScroll" "on"
              Option "CircularScrolling" "on"
              Option "CircScrollTrigger" "0"
              Option "EmulateTwoFingerMinZ" "40"
              Option "EmulateTwoFingerMinW" "8"
              Option "FingerLow" "30"
              Option "FingerHigh" "50"
              Option "MaxTapTime" "125"
 EndSection
```
## 重启gdm3
```bash
    sudo systemctl restart gdm3
```
