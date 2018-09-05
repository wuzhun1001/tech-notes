* [准备工作](#准备工作2)
* [系统烧录ESP32固件](#系统下烧录ESP32固件)
* [二 Ubuntu系统下烧录ESP32固件 ](二-Ubuntu系统下烧录ESP32固件)

* [三 Windows系统下烧录ESP32固件](准备工作2)

  



# ESP32固件烧录指南

**文档说明**：在进行TinyEngine JS的开发之前，必须先烧录固件让开发板/硬件支持TinyEngine功能。

所以本文将介绍如何在MAC/Windows/Ubuntu系统下烧录ESP32的TinyEngine固件。



### 准备工作2

ESP32使用USB转串口进行烧录，所以需要安装串口驱动：

* Windwos和MAC：点击该[地址]( https://cn.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)并下载相应系统对应驱动并安装。
* Ubuntu：无需安装驱动，默认自带。




### 系统下烧录ESP32固件

1） 安装python2.7 和 pip。

*  python2.7 下载地址： [下载地址](https://www.python.org/downloads/release/python-2715)   ，请选择对应系统的python2.7下载并安装。

*  pip安装：

  ```
  sudo easy_install pip
  ```

安装完成后，打开终端，输入`python —version`查看版本是否正确。



2）安装esptool.py

这是一个用python开发的针对ESP32的小工具，可以实现底层的操作,包括Flash的烧写，擦除。它也是一个开源项目，项目在github上进行托管, [托管地址](https://github.com/themadinventor/esptool)

安装方法：

```
pip install esptool
pip install pyserial
```

3）测试esptool.py是否生效

打开终端，输入`esptool.py` 查看是否有输出，如果esptool.py安装正常，会输出 【usage: esptool 】 类似信息。



4）擦除flash （**仅首次烧录需要**）

由于官方flash参数和分区表与TinyEngine定制固件可能不同，所以在您首次烧录TinyEngine固件时需要先擦除一遍Flash。以后再次更新TinyEngine时则不需要重复该操作了。

方法：

将ESP32的USB口和PC连接起来，然后在终端输入如下命令擦除Flash：

```
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 115200 erase_flash
```

FAQ:

注意这里/dev/tty.SLAB_USBtoUART是MAC上ESP32串口的默认端口号，如果出现找不到端口时，请确认ESP32的USB端口是否已经连接好，或使用`ls /dev/tty*` 查看是否有该端口或端口名称不同，如果端口名称不同，请使用正确的端口替换/dev/tty.SLAB_USBtoUART即可。



**5）使用esptool.py命令烧写TinyEngine固件**

进入到 TinyEngine项目的 firmware/esp32/esp32-devkitc目录，可以看到有如下文件

custom_partitions.bin：分区表

bootloader.bin： 启动文件

ota_data_initial： fota升级文件

tinyengine-esp32.bin： TinyEngine kernel固件。



**在终端输入如下命令开始烧写**：

```
esptool.py --chip esp32 --port /dev/tty.SLAB_USBtoUART --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0xd000 ota_data_initial.bin 0x1000 bootloader.bin 0x10000 tinyengine-esp32.bin 0x8000 custom_partitions.bin
```



FAQ：如前所述，如果端口找不到，请确认端口号正确，如firmware目录中的tinyengine固件名不是tinyengine-esp32.bin,请根据实际文件名替换tinyengine-esp32.bin即可。



### 二 Ubuntu系统下烧录ESP32固件

1） 安装python2.7 和 pip。

```
sudo apt-get install python2.7 python2.7-dev
sudo apt-get install python-pip
```

安装完成后，打开终端，输入`python —version`查看版本是否正确。



2）安装esptool.py

这是一个用python开发的针对ESP32的小工具，可以实现底层的操作,包括Flash的烧写，擦除。它也是一个开源项目，项目在github上进行托管, [托管地址](https://github.com/themadinventor/esptool)

在pip中使用如下命令安装esptool.py

```
python -m pip install esptool
```





 3) 安装pyserial串口通信工具。

```
python -m pip install pyserial
```



4）测试esptool.py是否生效

打开终端，输入`esptool.py` 查看是否有输出，如果esptool.py安装正常，会输出`usage: esptool [-h] [—chip {auto,esp8266,esp32}] [—port PORT] [--baud BAUD]` 类似信息。



5）擦除flash （**仅首次烧录需要**）

由于官方flash参数和分区表与TinyEngine定制固件可能不通，所以在您首次烧录TinyEngine固件时需要先擦除一遍Flash。以后再次更新TinyEngine时则不需要重复该操作了。

方法：

将ESP32的USB口和PC连接起来，然后在终端输入如下命令擦除Flash：

```
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 115200 erase_flash
```



FAQ:

注意这里/dev/ttyUSB0是MAC上ESP32串口的默认端口号，如果出现找不到端口时，请确认ESP32的USB端口是否已经连接好，或使用`ls /dev/ttyUSB*` 查看是否有该端口或端口名称不同，如果端口名称不同，请使用正确的端口替换/dev/ttyUSB0即可。



**5）使用esptool.py命令烧写TinyEngine固件**

进入到 TinyEngine项目的 [firmware/esp32/esp32-devkitc] 目录，可以看到有如下文件

custom_partitions.bin：分区表

bootloader.bin： 启动文件

ota_data_initial： fota升级文件

tinyengine-esp32.bin： TinyEngine kernel固件。



**在终端输入如下命令开始烧写**：

```
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0xd000 ota_data_initial.bin 0x1000 bootloader.bin 0x10000 tinyengine-esp32.bin 0x8000 custom_partitions.bin
```



FAQ：如前所述，如果端口找不到，请确认端口号正确，如firmware目录中的tinyengine固件名不是tinyengine-esp32.bin,请根据实际文件名替换tinyengine-esp32.bin即可。



### 三 Windows系统下烧录ESP32固件

1） 安装python2.7 

* 下载python2.7对应的windows安装包并安装，[地址](https://www.python.org/downloads/windows/)

安装完成后，打开cmd终端，输入`python —version`查看版本号是否正确，如果提示找不到python命令，请手动添加环境变量。



2） 安装pip。

* 下载地址是：https://pypi.python.org/pypi/pip#downloads ：

* 下载完成之后，解压到一个文件夹，用CMD控制台进入解压目录，例如我的目录是C盘work，则输入

  ````cd C:\work\pip-18.0 ```` (举例)

  最后输入命令安装：

  ```
  python setup.py install
  ```

  

FAQ: 安装好之后，我们直接在命令行输入pip，如果显示‘pip’不是内部命令，也不是可运行的程序。请手动添加环境变量。



3）安装esptool.py

这是一个用python开发的针对ESP32的小工具，可以实现底层的操作,包括Flash的烧写，擦除。它也是一个开源项目，项目在github上进行托管, [托管地址](https://github.com/themadinventor/esptool)

在pip中使用如下命令安装esptool.py

```
python -m pip install esptool
```



 3) 安装pyserial串口通信工具。

```
python -m pip install pyserial
```



4）测试python和esptool.py是否生效

打开终端，输入`python —version`查看版本是否正确。

打开终端，输入`esptool.py` 查看是否有输出，如果esptool.py安装正常，会输出【usage: esptool 】 类似信息。



5）擦除flash （**仅首次烧录需要**）

由于官方flash参数和分区表与TinyEngine定制固件可能不通，所以在您首次烧录TinyEngine固件时需要先擦除一遍Flash。以后再次更新TinyEngine时则不需要重复该操作了。

方法：

将ESP32的USB口和PC连接起来，查看**设备管理->端口->Slilicon Labs CP210x USBtoUART Bridge**的端口号，如我的电脑上是COM3，则在终端输入如下命令擦除Flash：

```
esptool.py --chip esp32 --port COM3 --baud 115200 erase_flash
```



FAQ:

如果出现找不到端口时，请确认ESP32的USB端口是否已经连接好和COM端口号是否正确。



**5）使用esptool.py命令烧写TinyEngine固件**

* 进入到 TinyEngine项目的`firmware/esp32/esp32-devkitc` 目录，可以看到有如下文件

custom_partitions.bin：分区表

bootloader.bin： 启动文件

ota_data_initial： fota升级文件

tinyengine-esp32.bin： TinyEngine kernel固件。

**备注**：windows进入目录的方法是：```cd 路径栏里面的复制路径```



* **在终端输入如下命令开始烧写**：

```
esptool.py --chip esp32 --port COM3 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0xd000 ota_data_initial.bin 0x1000 bootloader.bin 0x10000 tinyengine-esp32.bin 0x8000 custom_partitions.bin
```



FAQ：如前所述，如果端口找不到，请确认端口号正确，如firmware目录中的tinyengine固件名不是tinyengine-esp32.bin,请根据实际文件名替换tinyengine-esp32.bin即可。









