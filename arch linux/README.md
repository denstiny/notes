
<!-- vim-markdown-toc GFM -->

* [This my arch linux  Learning notes](#this-my-arch-linux--learning-notes)
	* [匹配需要需要配备码的蓝牙键盘](#匹配需要需要配备码的蓝牙键盘)
	* [中文字符乱码](#中文字符乱码)
	* [linux 网络共享](#linux-网络共享)
		* [如果你用的的ADSL上网的](#如果你用的的adsl上网的)
		* [如果你使用wifi上网的](#如果你使用wifi上网的)
		* [创建没有密码的wifi](#创建没有密码的wifi)
		* [如果你没有使用ADSL上网时(路由器LAN口直连)](#如果你没有使用adsl上网时路由器lan口直连)

<!-- vim-markdown-toc -->

# This my arch linux  Learning notes


## 匹配需要需要配备码的蓝牙键盘
> 切记：输入pair YourDeviceMacAddress后回车，屏幕可能会输出类似于以下的信息：
> [agent] Passkey: xxxxxx
> 这时候在你的蓝牙键盘上输入6位配对码后再回车即可完成配对。

```
# 安装bluez和bluez-utils
sudo pacman -S bluez
sudo pacman -S bluez-utils
# 进入蓝牙控制台
bluetoothctl
power on
agent on
default-agent
scan on
pair 键盘的MAC地址
trust 键盘的MAC地址
connect 键盘的MAC地址

```
> 如果以上方式使用不了试试下面这这种方式

```
sudo pacman -S blueberry # 图形化工具
```

## 中文字符乱码
1. 
```
sudo pacman -S wqy-zenhei ttf-fireflysung

```
<font size=4><b>安装之后设置中文字体</b></font>  

2. 
```
vim /etc/locale.gen
```
> 搜索 `zh_CN.UTF-8 UTF-8` 并取消注释
![20210314190910](https://i.loli.net/2021/03/14/XGJVxgFeq5T4sHW.png)

3.
```
locale-gen
locale
locale -a
```

## linux 网络共享

1. 安装`create_ap`
```sh
yay -S create_ap
```
2. 创建`wifi`

### 如果你用的的ADSL上网的
```sh
创建一个名字是wifiName，密码是wifiPasswd的热点
sudo create_ap wlp3s0 ppp0 wifiName wifiPasswd
```
### 如果你使用wifi上网的
```sh
创建一个名字是wifiName，密码是wifiPasswd的热点
sudo create_ap wlp3s0 wlp3s0 wifiName wifiPasswd
```
### 创建没有密码的wifi
```sh
创建一个名字是wifiName，没有密码的热点
sudo create_ap wlp3s0 wlp3s0 wifiName
```
### 如果你没有使用ADSL上网时(路由器LAN口直连)
```sh
创建一个名字是wifiName，密码是wifiPasswd的热点
sudo create_ap wlp3s0 enp4s0f2 wifiName wifiPasswd
```


