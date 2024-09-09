---
tags:
  - arduino
  - gpio
  - git
  - led
  - helloworld
---
Nobody starts learning a new programming language (or in this case, in a new target) without starting with a "Hello, World!". Normally, the "Hello, World" are small programs that run on the host or Personal Computer (PC) and display the phrase "Hello, World" on the screen. In our case, we don't have a screen! The equivalent of microcontrollers is to make an [LED](https://en.wikipedia.org/wiki/Light-emitting_diode) flash. We will take it a little further by making the LED turn on at our will using the button or push button on the Evaluation Board ([EVB](https://en.wikipedia.org/wiki/Microprocessor_development_board)).

In this practice we will learn how to create a program with Arduino. In this program we will use the fundamental pillar of microcontrollers: digital [[GPIO]]s. The use of digital GPIOs is present in countless devices and applications that range from detecting the pressing of buttons and generating indicators in user interfaces, to detecting or generating events in a biomedical device, such as detecting the presence of the sensor strip of a blood glucose meter or detecting the state of the power supply in a battery-powered Point-of-Care ([PoC](https://en.wikipedia.org/wiki/Point-of-care_testing)), among many other uses.
## Objectives
- Get to know the EVB STM32 Nucleo-F401RE.
- Learn about Arduino IDE and how it works.
- Create, compile and load the first program in Arduino.
- Use both input and output GPIOs.
- First contact with VCS (Git).
## Procedure
### Preparation of the IDE
#### Installation of Arduino IDE
The first thing we will do is install the application to develop Arduino-based programs: Arduino IDE. The acronym IDE responds to [Integrated Development Environment](https://en.wikipedia.org/wiki/Integrated_development_environment). An IDE is an application that provides the tools to develop, compile, implement and debug software. To install it, we will start by downloading the application from the following [link](https://www.arduino.cc/en/software). The version of Arduino IDE used for the preparation of these lab sessions has been 2.3.2.

>The application is available for multiple Operating Systems (OS). The practices can be carried out in all OSs compatible with the application, but the detail of its development, as well as the guarantee of its correct functioning, will be based on Windows. Specifically, Windows 10.

The installation of Arduino IDE is as easy as running the downloaded .exe file and using the mathematical algorithm Next, Next, Next, ..., and Finish. The detailed instructions on how to click on the different Next can be found at this [link.](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing/)
#### Configuration of the EVB
Once installed, Arduino IDE is able to program its own EVB: [Arduino UNO](https://store.arduino.cc/products/arduino-uno-rev3/), [Arduino Zero](https://store.arduino.cc/products/arduino-zero), [Arduino Due](https://store.arduino.cc/products/arduino-due), among others. However, the EVB that we will use is a [STMicroelectronics (STM)](https://www.st.com/) board: the [STM32 Nucleo-F401RE](https://www.st.com/en/evaluation-tools/nucleo-f401re.html). This EVB is compatible with Arduino; but, since it is not marketed by the Arduino company, the EVB configuration files must be imported into the IDE. Luckily for us, it is very easy to import the configuration files in Arduino IDE and STM provides the official configuration files for your EVB.

The procedure for importing the configuration files is located in one of the repositories of the official STM account on GitHub and you can find it at the following [link](https://github.com/stm32duino/Arduino_Core_STM32/wiki/Getting-Started).
### The EVB STM32 Nucleo-F401RE
As we have seen before, the EVB that we will use is the STM32 Nucelo-F401RE from STMicroelectronics. This EVB uses the STM32F401RET6U microcontroller from the same manufacturer. The interesting thing about EVBs is that they offer a quick way to prototype devices based on microcontrollers at a low cost. For example, the STM32 Nucleo-F401RE has a current retail price of about €15 and already integrates the debugger (electronic circuit necessary to program the microcontroller) into the EVB itself. Only a separate official debugger can already exceed €100. Obviously, the manufacturers create these EVBs to facilitate our entry into their development ecosystem at a low cost and thus be able to introduce their microcontrollers in our developments. Another advantage is that the EVB exposes all the pins of the microcontroller so that it facilitates connections with external elements during the prototyping phase and saves us having to manufacture our own prototyping plates (with which we save a significant cost in design, components, manufacturing and testing). In the following image you have a photograph of the EVB that we will use. Clicking on it will take you to an interactive infographic that will give you more information about what the EVB contains.

You can find the EVB schematic here. Three other important documents, but which we will not yet use in this practice with Arduino, are: the datasheet of the microcontroller, the reference manual of the microcontroller family and the user manual of the HAL (Hardware Abstraction Layer) libraries.