#安装备案
##linux版本
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

参考链接：
[下载conky主题](https://github.com/zagortenay333/Harmattan)
[更改.conkyrc 位置坐标](http://m.blog.csdn.net/article/details?id=52040186)

####ubuntu应用安装
[foxit pdf 阅读器](https://www.foxitsoftware.com/products/pdf-reader/) 使用起来比okular和xpdf都好
[酷狗音乐播放器](https://github.com/LiuLang/kwplayer-packages)
[remarkable](https://remarkableapp.github.io/linux.html) 写markdown ,直接下载deb包
[有道辞典](http://cidian.youdao.com/index-linux.html)
[Sublime](https://www.sublimetext.com/3)
[Texmarker]直接在软件中心搜索下载
