## FX503VD-Ventura13.6.3-OC.0.9.7-EFI <br>
# EFI 已经去掉三码，想要登录appleID 请自行生成三码<br>
ASUS FX63VD i5 7300HQ+HD630 Hackintosh<br>
主板型号：FX503VD           Bios版本：310<br>
硬件概况：CPU：i5-7300HQ    核显：UHD630  2048M  核显驱动正常<br>
独显：GTX1050 4G  内存：24G DDR4 2400  8G+16G 海力士<br>
有线网卡：RTL8111  无线网卡：Intel(R) Dual Band Wireless-AC 8265<br>
硬盘：威刚512G XPG GAMMIX S11L (固态硬盘)   HDD 1T<br>

功能实现概况：<br>
系统版本 MacOS Ventura13.6.3 <br>
无线正常驱动，蓝牙显示驱动但无法连接手机，屏幕亮度调节正常，系统音量调节正常<br>
键盘使用正常，鼠标使用正常，电池识别正常，USB定制各个接口均能正常识别U盘,睡眠唤醒正常。<br>
<br>
系统BUG和缺陷：<br>
1、独显GTX1050无法驱动（SSDT屏蔽） 10.13.6后N卡独显系统驱动无解。<br>
2、触控板无法驱动，暂时没有办法解决。<br>
3、蓝牙显示可以驱动，但是接连手机不稳定。<br>
~~4、声卡驱动BUG，mac系统音频播放时不时有电流声，win系统下重启进入MacOS 音乐播放会没有声音<br>
   只有滴答的电流声杂音，只有关机再开机进MacOS才会恢复正常，目前暂时没有办法解决~~<br>
5、无法设置OC启动为第一顺序启动系统，这样设置会导致开机卡死，尝试换固态和双硬盘双系统安装，但问题依旧。
   或许是华硕笔记本主板本身的问题吧，目前是只能利用启动快捷键ESC按键选择开机系统。<br>
6、隔空投送未能实现，后期可选择更换黑果无线蓝牙网卡实现 目前使用LocalSend可以实现投送功能替代<br>
7：HDMI视频输出，未测试，因为HDMI可能是独显直连，应该是无解。<br>
~~8：睡眠唤醒后声卡工作异常，音频输出失真，暂时无解，重启恢复正常。~~
<br>
EFI BUG完善记录：<br>
修复 Realtek ALC295 从 Windows 重启切换进入 macOS没有声音或失真的问题<br>
网上的解释：<br>
也就是说，Windows 下的 Realtek 驱动有问题，0x07 寄存器的正确的 COEF 值应该是 0x0f80，而该驱动将这个值设为了 0x2f80，于是导致 Windows 热重启到 macOS 或是 Linux 下扬声器无声。<br>
<br>
解决方法有两个，我只讲最简单的那个：<br>
卸载 Realtek 声卡驱动，切换为 Windows 自带的 HDA 驱动<br>
<br>
另一个方法是用 Codec Commander 来重设寄存器的值，不推荐此方法，在此不多引申了，感兴趣的朋友可以去原帖看看。<br>
<br>
参考：<br>
Tonymacx86.com - Sound loss after reboot back from Windows<br>
Tonymacx86.com - [solved] No audio after reboot from Windows (AppleHDA w/ALC 668)<br>
<br>
具体方法：<br>
1：事先卸载Realtek 声卡驱动，卸载驱动一定要勾选删除Realtek 声卡驱动程序，防止它自己偷偷自动安装回来导致修复失败<br>
2：win系统右键打开“此电脑”，选择“管理”，单击“设备管理器”>“声音、视频和游戏控制器”>“更新驱动程序”>“浏览计算机以查找驱动程序软件”>“从计算机的设备驱动程序列表中选择”，选择“高清晰度音频设备”（High Definition Audio），选择“下一步”，然后按照说明进行安装。然后在win重启再次选择进入macOS会发现之前在win系统切换进入macOS没有声音的问题解决了，但此方法仍然有小小瑕疵，就是在win系统下音频反而偶尔会出现类似电磁干扰的电流滋滋杂音，但总体比之前的macOS系统声音失真和睡眠唤醒导致macOS系统下声音失真异常要好很多了，算是目前比较好的一个解决方案了。<br>
