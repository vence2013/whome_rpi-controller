# whome-rpi-controller
系统有2种控制方式：网页直接控制（调试控制），自动池水过滤（包含灌溉）。

[Nodejs定时任务（node-schedule)](https://www.jianshu.com/p/8d303ff8fdeb)

### GPIO使用
rpio使用序号，比如16表示GPIO23。

PIN27: 水位检测
PIN28: 土壤检测

PIN15: 水泵开关
PIN16: 滴灌阀门

## 网页直接控制
1. 输出控制：水泵开关，滴灌开关。
2. 状态查询：水位检测，土壤检测。
如果水泵打开的，且水满，则关闭


## 自动池水过滤
比如每天进行3次：10:00, 13:00, 17:00。

状态显示：
当前时间，启动时间，剩余时间。

自动过滤过程：
1. 单次30分钟。
2. 间隔x1分钟启动抽水
3. 滴灌控制
    * 开始时清除符合标志。然后检测湿度，满足则设置符合标志
    * 如果未符合，则间隔x2分钟开启滴灌


---------

系统启动时启动服务

[Linux设置nodejs开机自启动(示例代码)](https://www.136.la/javascript/show-31954.html)
~~~
pi@raspberrypi:~/whome_rpi-controller $ sudo npm i pm2 -g
sudo pm2 start app.js --name="garden"
sudo pm2 list
sudo pm2 save
sudo pm2 startup
~~~
> 备注：一定要用root用户创建service，不然程序可能会遇到“Permission denied”错误。



[树莓派系统时间同步](https://www.cnblogs.com/imfanqi/p/4396292.html)
~~~
sudo timedatectl set-ntp true
sudo dpkg-reconfigure tzdata
~~~