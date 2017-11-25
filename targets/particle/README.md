### About

This folder contains files to run JerryScript beside Particle Device Firmware on Photon board.
It  runs a mini example, blinking an LED which is the "Hello World" example of the microcontroller universe.

这个文件夹包含在 `photon` 板卡上的 `Particle` 设备固件中运行的 `JerryScript` 文件。
它运行一个迷你示例，闪烁一个LED，这是微控制器领域的 `Hello World` 例子。

### How to build

#### 1. Preface / Directory structure 前言/目录结构

Assume `root` as the path to the projects to build.
The folder tree related would look like this.

假设“root”作为构建项目的路径。
文件夹树相关的是这样的：

```
root
  + jerryscript
  |  + targets
  |      + particle
  + particle
  |  + firmware
```


#### 2, Update/Install the Particle Firmware

For detailed descriptions please visit the official website of the firmware: https://www.particle.io/

You can checkout the firmware with the following command:

详细描述请访问固件的官方网站:https://www.particle.io/
您可以使用以下命令来检查固件:

```
# assume you are in root folder 假设您在根文件夹中
git clone https://github.com/spark/firmware.git particle/firmware particle/firmware
````

The Photon’s latest firmware release is hosted in the latest branch of the firmware repo.  

`Photon` 的最新固件发布在固件`repo`的最新分支中。

```
# assume you are in root folder
cd particle/firmware
git checkout latest
```

Before you type any commands, put your Photon in DFU mode: hold down both the SETUP and RESET buttons. Then release RESET, but hold SETUP until you see the RGB blink yellow. That means the Photon is in DFU mode. To verify that the Photon is in DFU mode and dfu-util is installed properly, try the dfu-util -l command. You should see the device, and the important parts there are the hex values inside the braces – the USB VID and PID, which should be 2b04 and d006 for the Photon.

在键入任何命令之前，将您的 Photon 设为 DFU 模式: 按住SETUP和RESET按钮，直到你看到RGB闪烁黄色，这意味着光子在DFU模式下。
为了验证 photon 在DFU模式下、dfu- util被正确安装，尝试 ` dfu-util -l ` 命令。
你应该看到设备，重要的部分是括号内的十六进制值——USB VID和PID，应该是2b04和d006的photon。

To build and flash the firmware: switch to the modules directory then call make with a few parameters to build and upload the firmware:

编译和烧写固件: 切换到 `firmware` 目录 ，然后调用一些参数来编译和上传固件:

```
cd modules
make PLATFORM=photon clean all program-dfu
```

#### 3. Build JerryScript

```
# assume you are in root folder
cd jerryscript
make -f ./targets/particle/Makefile.particle
```

This will create a binary file in the `/build/particle/` folder:
```
jerry_main.bin
```

That’s the binary what we’ll be flashing with dfu-util.


#### 4. Flashing

Make sure you put your Photon in DFU mode.
Alternatively, you can make your life a bit easier by using the make command to invoke dfu-util:

```
make -f targets/particle/Makefile.particle flash
```

You can also use this dfu-util command directly to upload your BIN file to the Photon’s application memory:

```
dfu-util -d 2b04:d006 -a 0 -i 0 -s 0x80A0000:leave -D build/particle/jerry_main.bin
```

#### 5. Cleaning

To clean the build result:
```
make -f targets/particle/Makefile.particle clean
```

### Running the example

The example program will blinks the D7 led periodically after the flash.