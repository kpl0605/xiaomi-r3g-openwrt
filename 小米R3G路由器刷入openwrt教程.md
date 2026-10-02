# 小米R3G路由器刷OPENWRT教程

## 前言

咸鱼花36CNY淘了一台二手小米R3G、带USB3.0接口、2LAN口、2.4G+5G双频

## 刷机步骤简述

1. 拿到机器插电开机，长按RESET键重置

2. 重置后，使用网线连接电脑，``192.168.31.1``进入后台配置管理员密码

3. 使用ssh破解工具直接取得临时ssh的root密码（账户root密码root）

4. 准备好finallshell与winscp

5. 下载R3G的breed固件

6. 使用winscp登录路由器上传breed固件到路由器/tmp目录

7. 使用finallshell进行ssh连接

8. ssh命令备份bootloader生成文件``bootloader.bin``到/tmp目录

9. 使用winscp将``bootloader.bin``拷贝到电脑备份

10. ssh命令刷入breed 1.2固件并自动重启，期间不得断电！

11. 等待2~3分钟后，路由器指示灯稳定时断开路由器电源

12. 断电下一直按住RESET键同时插上电源，等待路由器指示灯一直闪烁后松开reset键

13. 等待指示灯不再闪烁或者电脑识别到路由器后，``192.168.1.1``进入breed界面

14. breed界面下，点击固件备份，下载界面全部可以备份的文件

15. 在 breed 控制台的环境变量中，删除 ``normal_firmware_md5`` 这个键值、并新增``xiaomi.r3g.bootfw``的键值为`2`，点击保存

16. 准备好```kernel1```结尾的文件与``rootfs0``结尾的文件

17. 点击固件更新，闪存布局选``小米R3G openwrt ``并上传```kernel1```结尾的文件与``rootfs0``结尾的文件、选择自动重启、点击上传

18. 上传成功后、点击更新等待路由器自动重启，期间不得断电！

19. 等待路由器自动开机，电脑识别到路由器后，``192.168.5.1``进入openwrt的管理界面（账户root密码password）
    
    

## 详细教程

### 一、下载准备好所有需要使用的文件

1. finalshell 
   [FinalShell SSH工具,服务器管理,远程桌面加速软件,支持Windows,macOS,Linux,版本4.6.3,更新日期2025.5.21 - FinalShell官网](https://www.hostbuf.com/t/988.html)

2. winscp
   [WinSCP :: Official Site :: Download](https://winscp.net/eng/download.php)

3. breed固件
   https://breed.hackpascal.net/
   [breed-mt7621-xiaomi-r3g.bin](https://breed.hackpascal.net/breed-mt7621-xiaomi-r3g.bin)
   找到`breed-mt7621-xiaomi-r3g.bin`

4. ssh破解软件
   参见：[R3G 和 R3GV2 解锁 SSH-小米 / 红米无线路由器及网络设备-恩山无线论坛 - Powered by Discuz!](https://www.right.com.cn/forum/thread-8278054-1-1.html)
   https://wwri.lanzouo.com/i912T0p6b9sf  
   密码:at6h
   软件会被报毒

5. openwrt固件
   参见：[[2025.02.26] R3G OpenWrt 23.05.4稳定版 USB网络共享 MosDNS等 轻量自用包-小米 / 红米无线路由器及网络设备-恩山无线论坛 - Powered by Discuz!](https://www.right.com.cn/forum/thread-8345258-1-1.html)
   固件下载链接：https://www.123pan.com/s/Xt1Pjv-ysJgh.html 提取码:60jt
   管理地址192.168.5.1 
   账户root 
   密码password
   下载完成后需要解压，解压文件后会出现```kernel1```结尾的文件与``rootfs0``结尾的文件

       【注意：固件的分享链接里面是个文件夹，只需要下载一个版本就行，不需要下载1个多G的历史版本（你想下载也行）】

        

### 二、路由器首次插电开机并重置

1. 路由器不接网线，插电一分钟后用卡针长按reset键，等待约10秒路由器重置成功

2. 约1分钟后，使用网线连接电脑。电脑可能会识别到了路由器但是没有分配ip地址，可以进入网卡属性里面配置本机ip为192.168.31.2 网关填写192.168.31.1

3. 浏览器输入192.168.31.1回车进入路由器管理后台

4. 后台快速配置，管理员密码设置为12345678等简单密码【重点】

5. 无需配置其他选项，可以跳过的一直跳过即可

### 三、安装好finalshell与winscp

### 四、使用ssh破解软件破解ssh

如果使用官方方法解锁ssh有点繁琐，需要先刷入开发版固件才能解锁ssh还要绑定小米账号，隐私不友好，这里使用大神推荐的ssh破解工具

1. 点击运行``R3G_Get_SSH.exe``（温馨提示：因为软件利用漏洞攻击路由器破解ssh，所以会被杀毒软件报毒，请先退出杀毒软件。windowdefender请添加白名单或执行忽略）

2. 进入程序后，第一行输入192.168.31.1

3. 第二行输入你的管理员密码：12345678【你自己设置的】

4. 后面的问题一直回车不需要输入

5. 等待软件提示``done! Now you can connect to the router using several options: (user: root, password: root)``后，即已经破解

6. 当次破解后，不要重启路由器，否则开机后需要再次破解

### 五、使用finalshell与winscp进入路由器

#### Ⅰ.finalshell登入

1. 打开finalshell，点击“新标签页”旁边的📂

2. 在弹出的“连接管理器”，点击左上角白色文件夹符号，选择ssh连接

3. 在新建连接，名称自定义，主机填192.168.31.1，端口保持默认22，用户名root，密码root，点击确定

4. 在首页面双击服务器，如果有弹出是否接受密钥，请点击保存并接受

5. ssh连接成功后，客户端会显示ssh的彩蛋“ARE U OK”，代表成功连接

#### Ⅱ.winscp登入

1. 打开winscp

2. 点击新建节点，协议选SCP，主机名填写192.168.31.1，端口填写21，用户名root，密码root，点击登录

3. 登录成功后，找到tmp目录，把`breed-mt7621-xiaomi-r3g.bin`拖拽上传到此目录

4. 不要关闭软件

### 六、备份bootloader

1. 在ssh执行命令``dd if=/dev/mtd1 of=/tmp/Bootloader.bin``

2. 在winscp，找到tmp目录，右键刷新

3. 找到``Bootloader.bin``复制到电脑

### 七、刷入breed

在ssh执行命令``mtd write -r /tmp/breed-mt7621-xiaomi-r3g.bin Bootloader``

路由器自动重启，等待2分钟

### 八、进入breed

1. 断开路由器电源，断电下一直按住RESET键同时插上电源，等待路由器指示灯一直闪烁后松开reset键

2. 等待指示灯不再闪烁或者电脑识别到路由器后，``192.168.1.1``进入breed界面

3. 电脑可能会识别到了路由器但是没有分配ip地址，可以进入网卡属性里面配置本机ip为``192.168.1.2`` 网关填写``192.168.1.1``

4. 浏览器打开``192.168.1.1`` 出现Breed Web 恢复控制台 即证明成功刷入breed

### 九、备份固件并配置环境参数

1. breed界面下，点击固件备份，下载界面全部可以备份的文件

2. 在 breed 控制台的环境变量中，删除 ``normal_firmware_md5`` 这个键值、并新增``xiaomi.r3g.bootfw``的键值为`2`，点击保存

### 十、刷入openwrt

1. 准备好```kernel1```结尾的文件与``rootfs0``结尾的文件

2. 点击固件更新，闪存布局选``小米R3G openwrt ``并上传```kernel1```结尾的文件与``rootfs0``结尾的文件、选择自动重启、点击上传

3. 上传成功后、点击更新等待路由器自动重启，期间不得断电！

4. 等待路由器自动开机，电脑识别到路由器后，``192.168.5.1``进入openwrt的管理界面（账户root密码password）
   
   

## 注意事项

- 刷入任何文件时不得断开电源与网线，否则可能变砖

- 刷入前请先检查文件是否完整，防止文件损坏导致无法使用

- 无法访问``192.168.31.1``、``192.168.1.1``、``192.168.5.1``的原因可能是DHCP分配出错，可以自行设定ip地址

- 请勿使用不适配的路由器刷机

- 本方法同样适用于R3G v2（无USB口版）
  
  
