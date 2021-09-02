# vscode-skelly-stm32

VSCode STM32 skelleton base example project to work (develop and debug) with STM32 microcontrollers on a Linux system without the need of a full IDE.

This project example target a Nucleo-F401RE board, but the repository can be used as a reference to create new projects for other targets.

## Requirements

- Install required libraries:

```bash
sudo apt-get update
sudo apt-get -y install libncurses5 libncurses5-dev libncursesw5-dev
sudo apt-get -y install libc6 libc6-dev
sudo apt-get -y install libudev libudev-dev
```

- Install the [ARM Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads):

```bash
cd /opt
sudo wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.07/gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2
sudo tar -xf gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2
sudo rm -f gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2
sudo ln -sf /opt/gcc-arm-none-eabi-10.3-2021.07/bin/* /usr/bin/
```

- Install Debugging Tools:

```bash
sudo apt-get -y install openocd
sudo apt-get -y install stlink-tools
```

- In vscode, install C/C++ and Cortex-Debug extensions.


## How to Create a STM32 project supported by vscode

Here is the steps that has been followed to create current project:

1. Use [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) software to create a STM32 base project for a specific target, and generate it as a **"Makefile project"**.

2. Open the generated project directory inside vscode.

3. Create a **./vscode/settings.json** file and setup specific Toolchain and tools path in there.

4. Create a **./vscode/c_cpp_properties.json** file and setup C & C++ standard versions, the **arm-none-eabi-gcc compiler path**, **Defines** and the **compiler arguments** that are used in project *Makefile* to setup correctly intellisense functionality.

5. Create a **./vscode/tasks.json** file and setup Make build commands and tasks availables to be used from vscode.

6. Create a **./vscode/launch.json** file and setup each available debuggers to be used and their specific configurations for target device.

## Usage

Build and Debug:

1. Physically connect the hardware programmer to target device and the PC.

2. Open main.c file.

3. Go to **Run and Debug** left panel menu and select some launch configuration according to programmer and debugger that will be used (*stlink*, *jlink*, *openocd*...).

4. Press Run button.

Release Flash:

1. Press Ctrl+Shift+B to show the configured Tasks of the project.

2. Execute one of the Task to flash the firmware into the device through any of the supported and configured interfaces (**STFlash** for stlink, **JFlash** for jklink, **UARTFlash for UART interface).

## Notes

- Remember that compiler optimizations will translate the C/C++ code to an efficient memory/speed ASM, causing that lines execution behaviour when debugging will confuse a human. To keep a sequentially line-by-line execution a -Og flag/parameter needs to be passed to the compiler (change that in Makefile). Of course, you won't want this debug oriented optimization for Release, so you should use -Os for firmware Release.

- There is a [good video resource](https://www.youtube.com/watch?v=g2Kf6RbdrIs) where I have based this project setup from.
