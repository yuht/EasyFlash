<a href="https://996.icu"><img src="https://img.shields.io/badge/link-996.icu-red.svg" alt="996.icu" /></a>


[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)


# EasyFlash

---

## 1、介绍

EasyFlash是一款开源的轻量级嵌入式Flash存储器库，主要为MCU(Micro Control Unit)提供便捷、通用的上层应用接口，使得开发者更加高效实现基于的Flash存储器常见应用开发。该库目前提供 **三大实用功能** ：

- **Env** 让变量轻松实现掉电保存，支持 **写平衡** 模式，无需担心变量长度、磨损平衡等问题
 
使用 **键值对(key-value)** 方式将变量存储到Flash中。在产品上，能够更加简捷的实现 **设定参数** 或 **运行日志** 等信息掉电保存的功能。

- **IAP** 在线升级再也不是难事儿
 
该库封装了IAP(In-Application Programming)功能常用的接口，支持CRC32校验，同时支持Bootloader及Application的升级。

- **Log** 无需文件系统，日志可直接存储在Flash上

非常适合应用在小型的不带文件系统的产品中，方便开发人员快速定位、查找系统发生崩溃或死机的原因。同时配合[EasyLogger](https://github.com/armink/EasyLogger)(我开源的超轻量级、高性能C日志库，它提供与EasyFlash的无缝接口)一起使用，轻松实现C日志的Flash存储功能。

### 1.1、文件结构

|源文件                                 |描述   |
|:------------------------------        |:----- |
|\easyflash\src\flash_env.c             |Env（常规模式）相关操作接口及实现源码|
|\easyflash\src\flash_env_wl.c          |Env（磨损平衡模式）相关操作接口及实现源码|
|\easyflash\src\flash_iap.c             |IAP 相关操作接口及实现源码|
|\easyflash\src\flash_log.c             |Log 相关操作接口及实现源码|
|\easyflash\src\flash_utils.c           |EasyFlash常用小工具，例如：CRC32|
|\easyflash\src\flash.c                 |目前只包含EasyFlash初始化方法|
|\easyflash\port\flash_port.c           |不同平台下的EasyFlash移植接口及配置参数|
|\demo\stm32f10x\non_os                 |stm32f10x裸机的demo|
|\demo\stm32f10x\rtt                    |stm32f10x基于[RT-Thread](http://www.rt-thread.org/)的demo|
|\demo\stm32f4xx                        |stm32f4xx基于[RT-Thread](http://www.rt-thread.org/)的demo|

### 1.2、资源占用

```
最低要求： ROM: 6K bytes     RAM: 0.5K bytes + (Env大小)

Demo平台：STM32F103RET6 + RT-Thread 1.2.2 + Env(2K bytes)
实际占用： ROM: 6K bytes     RAM: 2.6K bytes
```

### 1.3、支持平台

目前已移植硬件平台有 `stm32f10x`与 `stm32f4xx` 系列的片内Flash，这个也是笔者产品使用的平台。其余平台的移植难度不大，在项目的设计之初就有考虑针对所有平台的适配性问题（64位除外），所以对所有移植接口都有做预留。移植只需修改 `\easyflash\port\flash_port.c` 一个文件，实现里面的擦、写、读及打印功能即可。

欢迎大家 **fork and pull request**([Github](https://github.com/armink/EasyFlash)|[OSChina](http://git.oschina.net/armink/EasyFlash)|[Coding](https://coding.net/u/armink/p/EasyFlash/git)) 。开源软件的成功离不开所有人的努力，也希望该项目能够帮助大家降低开发周期，让产品更早的获得成功。

## 2、流程

### 2.1、环境变量

下图为通过控制台（终端）来调用环境变量的常用接口，演示了环境变量 `"temp"` 从创建到保存，再修改，最后删除的过程。这些接口都支持被应用层直接调用。

![easy_flash_env](http://git.oschina.net/Armink/EasyFlash/raw/master/docs/zh/images/EnvDemo.gif)

### 2.2、在线升级

下图演示了通过控制台来进行IAP升级软件的过程，使用的是库中自带的IAP功能接口，演示采用的是串口+Ymodem协议的方式。你还也可以实现通过CAN、485、以太网等总线，来实现远程网络更新。

![easy_flash_iap](http://git.oschina.net/Armink/EasyFlash/raw/master/docs/zh/images/IapDemo.gif)

### 2.3、日志存储

下图过程为通过控制台输出日志，并将输出的日志存储到Flash中。重启再读取上次保存的日志，最后清空Flash日志。

![easy_flash_log](http://git.oschina.net/Armink/EasyFlash/raw/master/docs/zh/images/LogDemo.gif)

## 3、文档
具体内容参考`\docs\`下的文件。

## 4、许可
采用 GPL v3.0 开源协议，细节请阅读项目中的 LICENSE 文件内容。

---

## 1 Introduction

EasyFlash is an open source lightweight embedded flash memory library. It provide convenient application interface for MCU (Micro Control Unit). The developers can achieve more efficient and common application development based on Flash memory. The library currently provides **Three useful features** ：

- **Env(environment variables)** : Let variable easily achieve power down to save. Support **write balance mode** . No need to worry about variable length, wear leveling and other problems.
 
Use **key-value** model to stored variables to the Flash. You can be more simple to store **setting parameters** or **running logs** and other information which you want to power down to save.

- **IAP** : online upgrade is no longer a difficult thing.
 
The library encapsulates the IAP (In-Application Programming) feature common interface. Support CRC32 checksum. While supporting the bootloader and application upgrade.

- **Log** : The logs can store to product's flash which has no file-system.

It's very suitable for small without a file system products. The developer can easy to locate and query problem when system crashes or freezes. You can use [EasyLogger](https://github.com/armink/EasyLogger)( A super-lightweight, high-performance C log library which open source by me. It provides a seamless interface with EasyFlash) at the same time. So, it's so easy to store the logs to flash.

### 1.1 File structure

|Source file                            |Description   |
|:------------------------------        |:----- |
|\easyflash\src\flash_env.c             |Env (normal mode) interface and implementation source code.|
|\easyflash\src\flash_env_wl.c          |Env (wear leveling mode) interface and implementation source code.|
|\easyflash\src\flash_iap.c             |IAP interface and implementation source code.|
|\easyflash\src\flash_log.c             |Log interface and implementation source code.|
|\easyflash\src\flash_utils.c           |EasyFlash utils. For example CRC32.|
|\easyflash\src\flash.c                 |Currently contains EasyFlash initialization function only. |
|\easyflash\port\flash_port.c           |EasyFlash portable interface and configuration for different platforms.|
|\demo\stm32f10x\non_os                 |stm32f10x non-os demo.|
|\demo\stm32f10x\rtt                    |stm32f10x demo base on [RT-Thread](http://www.rt-thread.org/).|
|\demo\stm32f4xx                        |stm32f4xx demo base on [RT-Thread](http://www.rt-thread.org/).|

### 1.2 Resource consumption

```
Minimum : ROM: 6K bytes     RAM: 0.5K bytes + (Env size)

Demo    :STM32F103RET6 + RT-Thread 1.2.2 + Env(2K bytes)
Actual  : ROM: 6K bytes     RAM: 2.6K bytes
```

### 1.3 Supported platforms

Hardware platform has been ported `stm32f10x` and `stm32f4xx` series of on-chip Flash. This is my product platform. Remaining platform porting difficulty is little. The porting just modify `\easyflash\port\flash_port.c` file. Implement erase, write, read, print feature.

Welcome everyone to **fork and pull request**([Github](https://github.com/armink/EasyFlash)|[OSChina](http://git.oschina.net/armink/EasyFlash)|[Coding](https://coding.net/u/armink/p/EasyFlash/git)). The open source software success is inseparable from everyone efforts. I hope this project will help everyone reduce product development cycle and make product to success earlier.

## 2 Flow

### 2.1 Env

The figure below shows an ENV's common interface be called by the console(terminal). The ENV `"temp"` from creation to save, and then modify the final delete process. These interfaces are supported by the application layer called.

![easy_flash_env](https://raw.githubusercontent.com/armink/EasyFlash/master/docs/en/images/EnvDemo.gif)

### 2.2 IAP

The figure below shows the process of upgrade software through the console by IAP. It use this library comes with IAP function interface. Uses a serial port + Ymodem protocol mode. You can also be achieved through CAN, 485, Ethernet bus to online upgrade.

![easy_flash_iap](https://raw.githubusercontent.com/armink/EasyFlash/master/docs/en/images/IapDemo.gif)

### 2.3 Log

The following figure is the output log process through the console. The logs will save to flash at the same time. Then reboot the and read the logs from flash. At last will clean all logs which on flash.

![easy_flash_log](https://raw.githubusercontent.com/armink/EasyFlash/master/docs/en/images/LogDemo.gif)

## 3 Documents

All documents is in the `\docs\` folder.

## 4 License

Using GPL v3.0 open source license, please read the project LICENSE file.
