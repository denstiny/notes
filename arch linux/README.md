
<!-- vim-markdown-toc GFM -->

* [This my arch linux  Learning notes](#this-my-arch-linux--learning-notes)
	* [匹配需要需要配备码的蓝牙键盘](#匹配需要需要配备码的蓝牙键盘)
	* [中文字符乱码](#中文字符乱码)

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

