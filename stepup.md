#安装备案
##ubuntu版本
使用命令
```
file /bin/ls
```
本电脑是x86-64
##ns3
使用AODV example
```
cd Desktop/workspace/ns-allinone-3.26/ns-3.26/
 ./waf --run aodv --vis
```
##ubuntu显示亮度
不同电脑有acpi_video0和intel_backlight两种显示表示

对于acpi_video0的值范围在0-10之内，先用cat查看最大值
```
cat /sys/class/backlight/acpi_video0/max_brightness
```
再用
```
sudo -H gedit /etc/rc.local
```
修改文件中
```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
echo 0 > /sys/class/backlight/acpi_video0//brightness
exit 0
```
0是选择的值大小，可以设置从0到10。

当然有的电脑可能是使用intel_backlightt就把前面acpi_video0换成intel_backlight。
```
cat /sys/class/backlight/intel_backlight/max_brightness

sudo -H gedit /etc/rc.local

echo X > /sys/class/backlight/intel_backlight/brightness
```
参考链接：
[保存显示屏亮度](http://askubuntu.com/questions/151651/brightness-is-reset-to-maximum-on-every-restart)
## ubuntu耳机口亮红灯
问题在于数字接口 S/PDIF 16 是打开度原因，解决方案：
打开alsamixer
```
sudo alsamixer
```
找到S/PDIF 16，打开会是oo，使用m键手动关闭。

也可以直接再terminal使用命令进行关闭（IEC958是S/PDIF，不同S/PDIF不同的编号）
```
amixer set 'IEC958',16 off
```
有了命令之后就能设置开机自启动,使用startup application，输入开机启动度命令为
```
/usr/bin/amixer set 'IEC958',16 off
```
参考链接：
[命令行设置S/PDIF开关](http://askubuntu.com/questions/541847/is-there-any-way-to-save-alsamixer-settings-other-than-alsactl-store)
[找到S/PDIF 16对应的命令行代号是'IEC958',16](https://bbs.archlinux.org/viewtopic.php?id=192067)
[开机自启动](http://www.cnblogs.com/mo-wang/p/5153042.html)

##ubuntu 美化
####设置Numix主题
添加Numix源
```
$ sudo add-apt-repository ppa:numix/ppa
```
安装：
```
$ sudo apt update
$ sudo apt install numix-gtk-theme numix-icon-theme-circle
```
如果你想安装Numix壁纸，执行
```
$ sudo apt-get install numix-wallpaper-*
```
使用unity-tweak-tool设置主题和图标，执行如下命令安装
```
$ sudo apt install unity-tweak-tool
```
之后再unity-tweak-tool找到Numix设置为主题就可以了
安装oxygen cursor
```
sudo apt-get install oxygen-cursor-theme
sudo update-alternatives --config x-cursor-theme
```
选择cursor主题对应数字，一时间不会改变，需要logout再进去尽可以看到

参考链接：
[Numix主题安装](http://blog.topspeedsnail.com/archives/5886)

####使用conky
ubuntu 14.04直接apt-get install安装conky是1.9版本
如果需要使用conky1.10版本则先增加源更新后再安装
```
$ sudo add-apt-repository ppa:vincent-c/conky
$ sudo apt-get update
$ sudo apt-get install conky-all
```
建议再安装其他的部件
```
$ sudo apt-get install curl lm-sensors hddtemp
```
使用conky
```
$ conky -d
Conky: forked to background, pid is 11122
```
关闭conky
```
$ pkill conky
Conky: received SIGINT or SIGTERM to terminate. bye!
```
设置开机启动，打开startup application，增下如下设置
```
Field	Value
Name	conky
Command	conky -p 15
Comment	A system monitor
```
使用conky -p 15是因为保证conky在桌面准备好了时候再打开。
设置conky的文件就是在根目录～/下的.conkyrc文件。可以先使用conky默认配置文件。
```
$ cp /etc/conky/conky.conf ~/.conkyrc
```
之后修改.conkyrc就可以完成conky的实现了。
```
$ vi ~/.conkyrc
```
注意其中ctrl+H可以看隐藏文件，或者勾选view->Show hiden files。
参考链接：
[更新conky源](https://launchpad.net/~vincent-c/+archive/ubuntu/conky)
[安装conky并使用](http://www.shellhacks.com/en/HowTo-Install-and-Configure-Conky-in-Linux-Mint-Ubuntu-Debian)
####下载conky文件并修改
个人一开始编辑还是很难，下载的是[Harmattan](https://github.com/zagortenay333/Harmattan)主题，基本完全按照里面的步骤完成配置：

1. 把.harmattan-assets移动到 ~ 目录下

2. 在 .harmattan-themes 中找到各个分类最后的.conkyrc放到 ~ 目录下（只用选择一个，我选择Numix->God-mode->photo mode下的 .conkyrc文件）

3. 在[openweathermap](http://openweathermap.org/)网站中注册帐号，获取到一个API key填写到.conkyrc 中

4. 在网站中找到对应城市的ID编号写到.conkyrc 中

5. 文件在运行中出现错位，所以需要修改位置，修改gap_x=1100, gap_y=60 和voffset后面的值)

最后修改后的文件：
```
conky.config = {

-------------------------------------
--  Generic Settings
-------------------------------------
background=true,
update_interval=1,
double_buffer=true,
no_buffers=true,
imlib_cache_size=10,

draw_shades=false,
draw_outline=false,
draw_borders=false,
draw_graph_borders=false,
default_graph_height=26,
default_graph_width=80,
show_graph_scale=false,
show_graph_range=false,


-------------------------------------
--  Window Specifications
-------------------------------------
gap_x=1100,
gap_y=60,
minimum_height=620,
minimum_width=268,
own_window=true,
own_window_type="normal",
own_window_transparent=true,
own_window_hints="undecorated,below,sticky,skip_taskbar,skip_pager",
border_inner_margin=0,
border_outer_margin=0,
--alignment="middle_middle",
--own_window_argb_visual=true,
--own_window_argb_value=0,


-------------------------------------
--  Text Settings
-------------------------------------
use_xft=true,
xftalpha=1,
font="Droid Sans:size=8",
text_buffer_size=256,
override_utf8_locale=true,

short_units=true,
short_units=true,
pad_percents=2,
top_name_width=7,


-------------------------------------
--  Color Scheme
-------------------------------------
default_color="DCDCDC",
color1="DCDCDC",
color2="DCDCDC",
color3="DCDCDC",
color4="F9F9F9",
color5="D64937",
color6="888888",
color7="484848",
color8="2D2D2D",


-------------------------------------
--  API Key
-------------------------------------
template6="5737f72fbb86cd61a2117b07e8eb9d25",


-------------------------------------
--  City ID
-------------------------------------
template7="658226",



-------------------------------------
--  Temp Unit (default, metric, imperial)
-------------------------------------
template8="metric",


-------------------------------------
--  Locale (e.g. "es_ES.UTF-8")
--  Leave empty for default
-------------------------------------
template9=""

}


---------------------------------------------------
---------------------------------------------------


conky.text = [[
\
\
${execi 300 l=${template9}; l=${l%%_*}; curl -s "api.openweathermap.org/data/2.5/forecast/daily?APPID=${template6}&id=${template7}&cnt=5&units=${template8}&lang=$l" -o ~/.cache/forecast.json}\
${execi 300 l=${template9}; l=${l%%_*}; curl -s "api.openweathermap.org/data/2.5/weather?APPID=${template6}&id=${template7}&cnt=5&units=${template8}&lang=$l" -o ~/.cache/weather.json}\
\
\
\
\
${image ~/.harmattan-assets/misc/Numix/God-Mode/top-bg.png -p 20,30 -s 228x61}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bottom-bg.png -p 20,473 -s 228x119}\
${execi 300 cp -f ~/.harmattan-assets/photos/smallest/$(jq .weather[0].id ~/.cache/weather.json).png ~/.cache/weather.png}${image ~/.cache/weather.png -p 20,91 -s 228x86}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/border.png -p 20,91 -s 228x86}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-1.png -p 20,177 -s 228x86}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-2.png -p 20,263 -s 228x105}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-3.png -p 20,368 -s 228x105}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/bg-4.png -p 20,478 -s 228x14}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-v.png -p 95,185 -s 1x76}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-v.png -p 172,185 -s 1x76}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-h.png -p 33,369 -s 202x1}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/separator-h.png -p 33,269 -s 202x1}\
\
\
\
\
${color3}${voffset 187}${alignc 77}${execi 300 LANG=${template9} LC_TIME=${template9} date +%^a}${color}
${color3}${voffset -13}${alignc}${execi 300 LANG=${template9} LC_TIME=${template9} date -d +1day +%^a}${color}
${color3}${voffset -13}${alignc -77}${execi 300 LANG=${template9} LC_TIME=${template9} date -d +2day +%^a}${color}
\
\
\
\
${color2}${voffset 51}${alignc 77}${execi 300 jq -r .list[0].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[0].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
${color2}${voffset -13}${alignc}${execi 300 jq -r .list[1].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[1].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
${color2}${voffset -13}${alignc -77}${execi 300 jq -r .list[2].temp.min ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°/${execi 300 jq -r .list[2].temp.max ~/.cache/forecast.json | awk '{print int($1+0.5)}' # round num}°${color}
\
\
\
\
${goto 36}${voffset -171}${font Droid Sans :size=36}${color4}${execi 300 jq -r .main.temp ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num}°${font}${color}
${goto 46}${voffset 14}${font Droid Sans :size=12}${color4}${execi 300 jq -r .weather[0].description ~/.cache/weather.json | sed "s|\<.|\U&|g"}${font}${color}
${color4}${alignr 52}${voffset -73}${execi 300 jq -r .main.pressure ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num} hPa
${color4}${alignr 52}${voffset 7}${execi 300 jq -r .main.humidity ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num} %${color}
${color4}${alignr 52}${voffset 7}${execi 300 jq -r .wind.speed ~/.cache/weather.json | awk '{print int($1+0.5)}' # round num}${if_match "$template8" == "metric"} m/s${else}${if_match "$template8" == "default"} m/s${else}${if_match "$template8" == "imperial"} mi/h${endif}${endif}${endif}${color}
\
\
\
\
${voffset -117}${font Droid Sans Mono :size=22}${alignc}${color2}${time %H:%M}${font}${color}
${voffset 4}${font Droid Sans :size=10}${alignc}${color6}${execi 300 LANG=${template9} LC_TIME=${template9} date +"%A, %B %-d"}${font}${color}
\
\
\
\
${voffset 294}${goto 40}${color5}Cpu:${color}
${voffset 4}${goto 40}${color5}Mem:${color}
${voffset 4}${goto 40}${color5}Uptime:${color}
${voffset -47}${alignr 39}${color6}${cpu cpu0}%${color}
${voffset 4}${alignr 39}${color6}${memperc}%${color}
${voffset 4}${alignr 39}${color6}${uptime_short}${color}
${voffset -47}${alignc}${color2}${cpubar 5,36}${color}
${voffset 4}${alignc}${color2}${membar 5,36}${color}
${voffset 29}${goto 40}${loadgraph 26,190 D64937 D64937 -l}
\
\
\
\
${voffset 26}${goto 40}${color5}${top_mem name 1}${color}
${voffset 4}${goto 40}${color5}${top_mem name 2}${color}
${voffset 4}${goto 40}${color5}${top_mem name 3}${color}
${voffset 4}${goto 40}${color5}${top_mem name 4}${color}
${voffset 4}${goto 40}${color5}${top_mem name 5}${color}
${voffset -81}${alignc}${color2}${top_mem mem 1}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 2}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 3}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 4}%${color}
${voffset 4}${alignc}${color2}${top_mem mem 5}%${color}
${voffset -81}${alignr 39}${color6}${top_mem mem_res 1}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 2}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 3}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 4}${color}
${voffset 4}${alignr 39}${color6}${top_mem mem_res 5}${color}
${voffset -107}${goto 40}${color1}Proc${color}
${voffset -13}${alignc}${color1}Mem%${color}
${voffset -13}${alignr 39}${color1}Mem${color}
\
\
\
\
${if_existing /proc/net/route ppp0}
${voffset -227}${goto 40}${color5}Up: ${color2}${upspeed ppp0}${color5}${goto 150}Down: ${color2}${downspeed ppp0}
${voffset 10}${goto 40}${upspeedgraph ppp0 26,80 d64937 d64937}${goto 150}${downspeedgraph ppp0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup ppp0}${color5}${goto 150}Received: ${color2}${totaldown ppp0}
${else}
${if_existing /proc/net/route ppp1}
${voffset -240}${goto 40}${color5}Up: ${color2}${upspeed ppp1}${color5}${goto 150}Down: ${color2}${downspeed ppp1}
${voffset 10}${goto 40}${upspeedgraph ppp1 26,80 d64937 d64937}${goto 150}${downspeedgraph ppp1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup ppp1}${color5}${goto 150}Received: ${color2}${totaldown ppp1}
${else}
${if_existing /proc/net/route wlp2s1}
${voffset -253}${goto 40}${color5}Up: ${color2}${upspeed wlp2s1}${color5}${goto 150}Down: ${color2}${downspeed wlp2s1}
${voffset 10}${goto 40}${upspeedgraph wlp2s1 26,80 d64937 d64937}${goto 150}${downspeedgraph wlp2s1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlp2s1}${color5}${goto 150}Received: ${color2}${totaldown wlp2s1}
${else}
${if_existing /proc/net/route wlp2s0}
${voffset -266}${goto 40}${color5}Up: ${color2}${upspeed wlp2s0}${color5}${goto 150}Down: ${color2}${downspeed wlp2s0}
${voffset 10}${goto 40}${upspeedgraph wlp2s0 26,80 d64937 d64937}${goto 150}${downspeedgraph wlp2s0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlp2s0}${color5}${goto 150}Received: ${color2}${totaldown wlp2s0}
${else}
${if_existing /proc/net/route wlan0}
${voffset -279}${goto 40}${color5}Up: ${color2}${upspeed wlan0}${color5}${goto 150}Down: ${color2}${downspeed wlan0}
${voffset 8}${goto 40}${upspeedgraph wlan0 26,80 d64937 d64937}${goto 150}${downspeedgraph wlan0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlan0}${color5}${goto 150}Received: ${color2}${totaldown wlan0}
${else}
${if_existing /proc/net/route wlan1}
${voffset -292}${goto 40}${color5}Up: ${color2}${upspeed wlan1}${color5}${goto 150}Down: ${color2}${downspeed wlan1}
${voffset 10}${goto 40}${upspeedgraph wlan1 26,80 d64937 d64937}${goto 150}${downspeedgraph wlan1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup wlan1}${color5}${goto 150}Received: ${color2}${totaldown wlan1}
${else}
${if_existing /proc/net/route eth1}
${voffset -305}${goto 40}${color5}Up: ${color2}${upspeed eth1}${color5}${goto 150}Down: ${color2}${downspeed eth1}
${voffset 10}${goto 40}${upspeedgraph eth1 26,80 d64937 d64937}${goto 150}${downspeedgraph eth1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup eth1}${color5}${goto 150}Received: ${color2}${totaldown eth1}
${else}
${if_existing /proc/net/route eth0}
${voffset -318}${goto 40}${color5}Up: ${color2}${upspeed eth0}${color5}${goto 150}Down: ${color2}${downspeed eth0}
${voffset 10}${goto 40}${upspeedgraph eth0 26,80 d64937 d64937}${goto 150}${downspeedgraph eth0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup eth0}${color5}${goto 150}Received: ${color2}${totaldown eth0}
${else}
${if_existing /proc/net/route enp0s0}
${voffset -331}${goto 40}${color5}Up: ${color2}${upspeed enp0s0}${color5}${goto 150}Down: ${color2}${downspeed enp0s0}
${voffset 10}${goto 40}${upspeedgraph enp0s0 26,80 d64937 d64937}${goto 150}${downspeedgraph enp0s0 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup enp0s0}${color5}${goto 150}Received: ${color2}${totaldown enp0s0}
${else}
${if_existing /proc/net/route enp0s1}
${voffset -344}${goto 40}${color5}Up: ${color2}${upspeed enp0s1}${color5}${goto 150}Down: ${color2}${downspeed enp0s1}
${voffset 10}${goto 40}${upspeedgraph enp0s1 26,80 d64937 d64937}${goto 150}${downspeedgraph enp0s1 26,80 d64937 d64937}
${voffset 9}${goto 40}${color5}Sent: ${color2}${totalup enp0s1}${color5}${goto 150}Received: ${color2}${totaldown enp0s1}
${else}
${voffset -311}${goto 40}${color5}Network disconnected${color}
${image ~/.harmattan-assets/misc/Numix/God-Mode/offline.png -p 44,284 -s 16x16}
${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}${endif}
\
\
\
\
${image ~/.harmattan-assets/misc/Numix/God-Mode/pressure.png -p 224,95 -s 16x16}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/humidity.png -p 224,115 -s 16x16}\
${image ~/.harmattan-assets/misc/Numix/God-Mode/wind-2.png -p 224,136 -s 16x16}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[0].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-1.png}${image ~/.cache/weather-1.png -p 41,207 -s 32x32}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[1].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-2.png}${image ~/.cache/weather-2.png -p 119,207 -s 32x32}\
${execi 300 cp -f ~/.harmattan-assets/icons/#dcdcdc__32/$(jq .list[2].weather[0].id ~/.cache/forecast.json).png ~/.cache/weather-3.png}${image ~/.cache/weather-3.png -p 195,207 -s 32x32}${font}${voffset -120}\
]]
```
其中API key选项需填写申请的API key，在申请的账户里能够找到。

参考链接：
[下载conky主题](https://github.com/zagortenay333/Harmattan)
[更改.conkyrc 位置坐标](http://m.blog.csdn.net/article/details?id=52040186)
#### compiz 3D动态
之前在CentOS系统使用过，最常见就是果冻特效和3D切换。但ubuntu安装一次点错选项直接图形界面就崩了，权利太大不建议使用。
####ubuntu应用安装
[foxit pdf 阅读器](https://www.foxitsoftware.com/products/pdf-reader/) 使用起来比okular和xpdf都好
[酷狗音乐播放器](https://github.com/LiuLang/kwplayer-packages)
[remarkable](https://remarkableapp.github.io/linux.html) 写markdown ,直接下载deb包
[有道辞典](http://cidian.youdao.com/index-linux.html)
[Sublime](https://www.sublimetext.com/3) 
[Texmaker](http://www.xm1math.net/texmaker/) 可直接在软件中心搜索下载

##Ubuntu壁纸
设置一个随时间变化的壁纸，也就算wallpaper slideshow。
有很多软件能够实现动态变化的壁纸，可以安装[wallch](http://www.omgubuntu.co.uk/2016/12/8-bit-day-wallpaper-changes-day) 和 [variety](http://ubuntuhandbook.org/index.php/2016/01/install-variety-wallpaper-changer-in-ubuntu-16-04/)，也可以直接用ubuntu自带的[shotwell](http://askubuntu.com/questions/134/how-do-i-create-a-desktop-wallpaper-slideshow)来直接将slideshow设置为桌面背景。
启发于一个随一天时间变化的壁纸 [A Day in the Life](http://barid42.deviantart.com/art/A-Day-in-the-Life-204881196),下载了它的壁纸。但运行install安装并没有反应，查看install.sh文件，发现路径与14.04的壁纸所在文件已经不同了，需要手动配置，这样不需要借助其他应用来修改壁纸。

在ubuntu 14.04需要两个xml配置文件，一个作为入口路径，一个是配置文件。
1、入口路径文件在/usr/share/gnome-background-properties下，新建 MyfirstSlideShow.xml文件(可参考trusty-wallpapers.xml文件写法)
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE wallpapers SYSTEM "gnome-wp-list.dtd">
<wallpapers>
  <wallpaper deleted="false">
    <name>Daily Wallpapers</name>
    <filename>/usr/share/backgrounds/wallpaper/MyFirstSlideShow.xml</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>01</name>
    <filename>/usr/share/backgrounds/wallpaper/01.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>02</name>
    <filename>/usr/share/backgrounds/wallpaper/02.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>03</name>
    <filename>/usr/share/backgrounds/wallpaper/03.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>04</name>
    <filename>/usr/share/backgrounds/wallpaper/04.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>05</name>
    <filename>/usr/share/backgrounds/wallpaper/05.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>06</name>
    <filename>/usr/share/backgrounds/wallpaper/06.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>07</name>
    <filename>/usr/share/backgrounds/wallpaper/07.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
  <wallpaper>
    <name>08</name>
    <filename>/usr/share/backgrounds/wallpaper/08.png</filename>
    <options>zoom</options>
    <pcolor>#000000</pcolor>
    <scolor>#000000</scolor>
    <shade_type>solid</shade_type>
  </wallpaper>
</wallpapers>
```
配置文件和壁纸都放在/usr/share/backgrounds目录下的wallpaper文件夹中，
新建配置文件MyFirstSlideShow.xml(参考contest文件下 trust.xml写法)
```
<background>
<starttime>
<year>2016</year>
<month>12</month>
<day>16</day>
<hour>0</hour>
<minute>0</minute>
<second>01</second>
</starttime>

<static>
<duration>18000.0</duration>
<file>/usr/share/backgrounds/wallpaper/08.png</file>
</static>
<!-- it is now 5am -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/08.png</from>
<to>/usr/share/backgrounds/wallpaper/01.png</to>
</transition>
<!-- it is now 7am -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/01.png</from>
<to>/usr/share/backgrounds/wallpaper/02.png</to>
</transition>
<!-- it is now 9am -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/02.png</from>
<to>/usr/share/backgrounds/wallpaper/03.png</to>
</transition>
<!-- it is now 11am -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/03.png</from>
<to>/usr/share/backgrounds/wallpaper/04.png</to>
</transition>
<!-- it is now 1pm -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/04.png</from>
<to>/usr/share/backgrounds/wallpaper/05.png</to>
</transition>
<!-- it is now 3pm -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/05.png</from>
<to>/usr/share/backgrounds/wallpaper/06.png</to>
</transition>
<!-- it is now 5pm -->
<transition type="overlay">
<duration>7200.0</duration>
<from>/usr/share/backgrounds/wallpaper/06.png</from>
<to>/usr/share/backgrounds/wallpaper/07.png</to>
</transition>
<!-- it is now 7pm -->
<transition type="overlay">
<duration>7200.0</duration>
<from>//usr/share/backgrounds/wallpaper/07.png</from>
<to>/usr/share/backgrounds/wallpaper/08.png</to>
</transition>
<!-- it is now 9pm -->
<static>
<duration>10800.0</duration>
<file>//usr/share/backgrounds/wallpaper/08.png</file>
</static>
<!-- it is now midnight -->
</background>

```
保存，就能在System Setting->Apperance中找到对应的daily theme,右上方也会有daily theme图标的提示。

TODO: 写个安装脚本

##Ubuntu network
每次登录都收到 “Network service discovery disabled. Your current network has a .local domain, which is not recommended and incompatible with the Avahi network service discovery. The service has been disabled.”的信息。为了取消提示，需要禁用检测。
首先进入root
```
sudo -i
gedit /etc/default/avahi-daemon
```
增加这行或在修改值为0。
```
AVAHI_DAEMON_DETECT_LOCAL=0
```
参考链接：
[http://askubuntu.com/questions/339702/network-service-discovery-disabled-what-does-this-mean-for-me](http://askubuntu.com/questions/339702/network-service-discovery-disabled-what-does-this-mean-for-me)


##Ubuntu shell
####编写脚本
打开文本编辑器,写入
```
#!/bin/bash
echo "Hello World !"
```
保存为test.sh
其中，" #! " 是一个约定的标记， 它告诉系统这个脚本需要什么解释器来执行， 即使用哪一种Shell。
####运行脚本：
1、使用可执行程序

查看文件属性
```
# ls -l test.sh #r可读、w可写、x表示可执行权限
```
在终端中test.sh所在目录下输入
```
chmod +x ./test.sh #使脚本具有执行权限
./test.sh #执行脚本
```
注意， 一定要写成./test.sh， 而不是test.sh， 运行其它二进制的程序也一样， 直接写test.sh，linux系统会去PATH里寻找有没有叫test.sh的， 而只有/bin, /sbin, /usr/bin， /usr/sbin等在PATH里， 你的当前目录通常不在PATH里， 所以写成test.sh是会找不到命令的， 要用./test.sh告诉系统说， 就在当前目录找。

2、直接调用解释器

这种运行方式是， 直接运行解释器， 其参数就是shell脚本的文件名。
```
/bin/sh test.sh
```
这种方式运行的脚本， 不需要在第一行指定解释器信息， 写了也没用。

####卸载sh安装的程序
为了安装随天气变化壁纸，下载[Weatherpaper](http://www.omgubuntu.co.uk/2010/08/weatherpaper-puts-the-weather-outside-on-your-desktop-inside),使用里面install.sh安装，但运行失败，需要卸载。
在使用脚本度基础上，直接查看install.sh文件
```
PROGRAM_FOLDER="/opt/WeatherPaper"

# Make sure only root can run our script
if [ "$(id -u)" != "0" ]; then
   echo "This script must be run as root" 1>&2
   exit 1
fi

# Make the program folder
if [ ! -d "$PROGRAM_FOLDER" ]; then
    mkdir "$PROGRAM_FOLDER"
fi
    
# Copy files
cp settings.cfg "$PROGRAM_FOLDER/"
cp tango.zip "$PROGRAM_FOLDER/"
cp weatherpaper.py "$PROGRAM_FOLDER/"
cp first-run.sh "$PROGRAM_FOLDER/"
cp weatherpaper.sh "$PROGRAM_FOLDER/"
cp editor.pyw "$PROGRAM_FOLDER/"
cp SettingsPage.xml "$PROGRAM_FOLDER/"
cp pywapi.py "$PROGRAM_FOLDER/"
cp zipfile.py "$PROGRAM_FOLDER/"
cp arialbd.ttf "$PROGRAM_FOLDER/"
cp LICENSE.txt "$PROGRAM_FOLDER/"
cp README.txt "$PROGRAM_FOLDER/"
cp weather-few-clouds.png "$PROGRAM_FOLDER/"
cp weatherpaper.desktop "$PROGRAM_FOLDER/"
cp weatherpaper-editor.desktop "$PROGRAM_FOLDER/"

# Add an entry to the Gnome/KDE Menu
chmod a-x "$PROGRAM_FOLDER/weatherpaper.desktop"
chmod a-x "$PROGRAM_FOLDER/weatherpaper-editor.desktop"
cp "$PROGRAM_FOLDER/weatherpaper.desktop" "/usr/share/applications/weatherpaper.desktop"
cp "$PROGRAM_FOLDER/weatherpaper-editor.desktop" "/usr/share/applications/weatherpaper-editor.desktop"

echo "Installation complete."
echo "To run WeatherPaper, go to Applications->Accessories->WeatherPaper"
```
发现其实就是拷贝文件到/opt/WeatherPaper文件中，因而进入/opt目录下查看到WeatherPaper目录，删除目录就可以了
```
sudo rm -r WeatherPaper
```
注意终端使用cd /opt和cd opt是不同路径，一个是root目录下，一个是在用户home目录下。
#### ubuntu设置变量值
在bash中使用[变量](http://cn.linux.vbird.org/linux_basic/0320bash.php#variable)能够更方便管理。
可以通过echo输出各变量表示值，bash 中不存变量就输出空，否则输出对应值
```
echo $variable
echo $PATH
```
通过=就能给变量赋值啦，给PATH增加路径可以，直接在终端输入
```
 PATH=$PATH:/home/dmtsai/bin
 PATH="$PATH":/home/dmtsai/bin
 PATH=${PATH}:/home/dmtsai/bin #三者也可以
```	
使用env可以查看目前系统中默认环境变量,set 看所有的环境变量，包括自定义
```
env
set #或着declare
```

变量的方便用途就是简化路径名，如工作路径太长
```
work="/cluster/server/work/taiwan_2005/003/"
 cd $work
```
目前work只对该bash子程序有用，要使所有有用就要升级为环境变量。
使用export [变量名] 就能够给子程序使用，即升级为环境变量. 或者使用declare语句
```
export work 
#或者 declare -x work
```

查看环境变量是否又增加work变量
```
export | grep work
```
使用declare+x就能取消自定义环境变量
```
declare +x work
```
注意设置变量为空，用unset [变量名],使用变量名=""或变量名=都是表示空字符串。

TODO:新开一个终端就自定义的环境变量就没有了，是不是需要写个脚本。
####设置命令别名
简化命令，可以用一个别名代替。比如用la代替显示所有文件-a详细信息-l。
```
alias lm='ls -al |more'
```
查看所有别名可以输入alias。使用unalias可以取消别名
```
unalias lm
```
命令别名是命令，而变量只是代替值，需要echo输出值。
####Bash Shell配置文件
bash文件主要在~和/etc目录下的隐藏文件(.开头文件)，可以通过ls -al查找文件或着者ctrl+H，用cat或者编辑器查看文件。
1、进站信息文件 /etc/issue
2、环境配置文件 /etc/profile（只有login shell才读）里面会调用~/.bashrc （non-login shell会读，终端使用就是non login）
3、命令存储历史 ~/.bash_history
4、退出bash操作 ~/.bash_logout

####数据重导向
[数据重导向](http://cn.linux.vbird.org/linux_basic/0320bash.php#redirect_redirect)用于把终端的输出数据转到文本文件输出，相当于log文件

标准输入　　(stdin) ：代码为 0 ，使用 < 或 << ；

标准输出　　(stdout)：代码为 1 ，使用 > 或 >> ；

标准错误输出(stderr)：代码为 2 ，使用 2> 或 2>> ；
```
ll
ll  > Desktop/test/testlog #导向文件，终端不显示
cat  Desktop/test/testlog
```
关于一个>与两个>>含义:

1> ：以覆盖的方法将『正确的数据』输出到指定的文件或装置上；

1>>：以累加的方法将『正确的数据』输出到指定的文件或装置上；

2> ：以覆盖的方法将『错误的数据』输出到指定的文件或装置上；

2>>：以累加的方法将『错误的数据』输出到指定的文件或装置上；

< :将原本需要由键盘输入的数据，改由文件内容来取代的意思

<< :后面接结束时所需输入字符,而不需要ctrl+d离开

实现输出正确找到的信息与错误信息（像permission deny）输出到文件中。
注意：
1、垃圾桶黑洞装置/dev/null 可以吃掉任何导向这个装置的信息.

2、写入统一文件
```
find /home -name .bashrc > list 2> list  <==错误
find /home -name .bashrc > list 2>&1     <==正确
find /home -name .bashrc &> list         <==正确
```
由于两股数据同时写入一个文件，又没有使用特殊的语法， 此时两股数据可能会交叉写入该文件内，造成次序的错乱。

####管线命令
处理必须前面命令必须正确的连续命令，命令之间用|符号隔开，满足如下两点：
1、管线命令仅会处理 standard output，对于 standard error output 会予以忽略

2、管线命令必须要能够接受来自前一个命令的数据成为 standard input 继续处理才行。

配合命令：

1、cut

2、grep 不输出空白行和注释的行方法grep -v '^$' [filename]| grep -v '^#'

3、sort

4、uniq 不显示重复行（该行只跟上一行判断，一样就不显示,所以一般配合sort，
将一样的先列一块）

5、wc 统计字、行、字符

6、tee 双向导向，即又输出终端又输出文件

7、- 减号在管线中将前面命令输出作为输入

####查看程序资讯
每个程序（process）会依据UID/GID触发后，会被分配一个PID
使用ps -l命令查看程序UID/GID PID PPID(parent ID)
例如，在bash中再启用一个子bash，子bash的PPID就是父bash的PID
```
liu@liu-Lenovo-IdeaPad-Y470:~$ ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000  4857  3408  0  80   0 -  6848 wait   pts/1    00:00:00 bash
4 R  1000  4871  4857  0  80   0 -  3552 -      pts/1    00:00:00 ps
liu@liu-Lenovo-IdeaPad-Y470:~$ bash
liu@liu-Lenovo-IdeaPad-Y470:~$ ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000  4857  3408  0  80   0 -  6848 wait   pts/1    00:00:00 bash
0 S  1000  4876  4857  1  80   0 -  6850 wait   pts/1    00:00:00 bash
4 R  1000  4897  4876  0  80   0 -  3552 -      pts/1    00:00:00 ps
```
使用ps -aux能够看到所有系统程序。
除此之外，我们必须要知道的是僵尸 (zombie) 程序是什么？ 通常，造成僵尸程序的成因是因为该程序应该已经运行完毕，或者是因故应该要终止了， 但是该程序的父程序却无法完整的将该程序结束掉，而造成那个程序一直存在内存当中。 如果你发现在某个程序的 CMD 后面还接上 <defunct> 时，就代表该程序是僵尸程序啦

关闭程序 使用kill -signal PID命令。其中signal表示数字，9强制终止，15为正常方式关闭。
‘’‘
kill -9 PID
’‘’
删除父程序子程序就全删除啦,使用pstree -A 能够更好的看到相关性。加上|grep能更快找到对应的程序

####job control的管理
job对应于前面process，job仅仅是在bash这个process上运行的工作程序，范围更小，同时可以在bash中管理。
将命令放到背景中运行 &
```
conky &
```
此时会分配一个[job number]同时后面接上PID，并显示在界面中。
查背景环境中的job
```
jobs -l
```
使用fg能够拿到前景
```
fg %jobnumber
```
使用kill能够删除job
```
kill -9 %jobnumber #强制删除
kill -15 %jobnumber #正常退出
```