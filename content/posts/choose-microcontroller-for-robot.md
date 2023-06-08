---
title: "How to Choose a Microcontroller for your Robot"
date: 2022-07-06
draft: false
banner: 'choose-microcontroller-for-robot.jpg'
summary: "Which is the right microcontroller for my robot? Are there any gotchas I need to watch for?"
math: false
categories: ['aspiring']
tags: ['microcontroller','electronics']
---

## What is a Microcontroller?
A microcontroller is a tiny computer chip that controls everything that is going on inside a robot. A microcontroller is the brain of your robot. It helps the robot move, recognize its surroundings, and respond to the environment. This means that a microcontroller is the most important part of a robot. It controls the things that make your robot act like a robot. It makes sure that your robot moves smoothly and that it can respond to the environment properly. A microcontroller is very small, and it needs to be small enough to fit in the robot.

### Microcontroller vs Microprocessor
Microprocessors are used in computers, and they are generally much bigger than microcontrollers. Microcontrollers are generally packed with peripherals that will help it to talk with other devices like analog to digital converters (ADC), digital to analog converters (DAC), general purpose IO (GPIO) pins, etc making them easier to connect to other devices. Microprocessors on the other hand are specialized to perform processing alone but at a very fast pace. They are typically mounted on a motherboard where other peripherals are connected to complete the functionality.

Microcontrollers are smaller and cheaper. One thing you should know about microcontrollers is that they are easy to program. In the recent years, frameworks like Arduino has made it even easier. Microprocessors are much harder to program than microcontrollers. This is because microprocessors need to be able to handle complex tasks. 

### RISC vs CISC
There are two kinds of architectures: the RISC and the CISC. RISC stands for Reduced Instruction Set Computer and CISC stands for Complex Instruction Set Computer. RISC is simpler than CISC because it has fewer instructions. A RISC microcontroller is usually smaller, has less code space, and has less RAM than a CISC microcontroller. In addition, it is usually faster and uses less power. On the other hand, CISC has more instructions and hence a larger instruction set. A CISC microcontroller is usually larger than a RISC microcontroller. Moreover, it requires more code space and more RAM. The RISC is faster because the processor processes one instruction at a time. The CISC is slower because it processes multiple instructions simultaneously. 

Because of the reduced demands and simplicity of the RISC architecture, almost all microcontroller are RISC based. The older Intel microcontrollers like 8085 are CISC based.Whereas all the latest microcontrollers like PICs, most arduinos (AVR, ARM, etc) and ESP32s are based on RISC architecture. 

### Bits 
You might have seen terms like 8-bit, 16-bit or 32-bit in the specs of a microcontroller. This number refers to the size of the registers in the microcontroller. A 8 bit microcontroller has a  8 bit registers. It means that each byte can hold eight binary digits or bits. Each bit of the byte represents the status of a single switch. For example, if there is a switch in a state that represents the number 2, this will be represented by the 1st bit in the byte being set to 1. The 2nd bit would be set to 0, and the 3rd bit would be set to 0. All the bits represent the status of the switch. The byte is used for storing the status of a single switch. 

8-bit or 16-bit microcontrollers are cheap and easy to work with.  They have smaller clock cycles in the order of MegaHertz compared to GigaHertz for 32-bit microcontrolelrs. This makes them easier to prototype even using breadboard without the need for any super precise setup. 

Choosing a microcontroller for your robot is important. You must make sure that the microcontroller you use is compatible with the rest of the components of your robot. You must also make sure that your microcontroller has all the features you need. Next we will look at some factor to consider before choosing a microcontroller.

## Factors to consider before choosing a microcontroller
### Requirements
The first question you need to answer is “what do I want my robot to do?”  Before you go out and buy a microcontroller, you should ask yourself what the final purpose of your robot is. It’s a good idea to write a description of what the robot is capable of doing, so that you know exactly what features you need to have. You should also write down what features you do not need. You must also make sure that the microcontroller you use is compatible with the rest of the components of your robot. Some features of the microcontroller that you should think about include memory, the number of input/output pins, clock speed, and how much power it takes. 

### General Purpose Input/Output (GPIO) Pins
One of the major determining factors is the GPIO pins. These are the pins that you connect to sensors and motors. If you need to control multiple sensors or motors, you need to have a lot of these pins. If the microcontroller you are considering does not have enough GPIO pins to connect all your devices, you will need to use a separate board to connect all those sensors and motors which will be lot more work. That is also the main reason why you need to write down the functionality of the robot upfront so that you account for all the sensors and actuators and have no surprises in the end. 

### Peripherals
Peripherals in a microcontroller are the things like UARTs (Universal Asynchronous Receiver Transmitters), timers, ADC (Analog-to-Digital Converters), SPI (Serial Peripheral Interface) and so on. Peripherals will let you connect sensors and other devices to your microcontroller easily.

The first consideration you need to take into account is whether the microcontroller has peripherals that are compatible with the ones you have in your robot. For example, if your robot has an inertial measurement unit (IMU) with a SPI interface, you need to make sure your microcontroller has one too. If not, you will need to use extra shields or boards to connect the IMU will is not ideal. So, you need to make sure that you choose the one that has the all the peripherals you need.

### Memory
Memory in a microcontroller is the space on the chip that is used for storing information. This includes instructions, data, and other things. Program code is stored in a memory location and can be accessed with a set of instructions called a program counter. Data is stored in another memory location and can be read and written with data instructions. Some microcontrollers have only program memory while others have both program and data memories. The amount of memory available in a microcontroller varies greatly. The most common size of program memory is 256K. You can usually get a microcontroller with 512K, 1M, 2M, 4M and 8M. Some microcontrollers can have up to 32M. It is important to have enough of it. If your microcontrollers have a limited amount of memory, you can use external storage to expand the memory. However this is not the ideal option and should not be your first choice. The main consideration here is how much memory you need for your project. A general rule of thumb is that the more complex your robot is, the more memory you need. The microcontroller needs to know what it is doing and how it is supposed to be performing. So, you should choose the microcontroller that gives you the most room for expansion. 

### Clock Speed
The clock speed is the frequency at which the microprocessor runs and therefore it controls how fast instructions can be executed. The faster the microprocessor runs, the more instructions it can execute per second. This can mean that the microprocessor can run more complex calculations and applications at a higher speed. Clock speed is one of the indicators on how much compute power your robot has. Depending on what you want your robot to do, the compute power will be different. For example, for a line following robot, if you are using an IR sensor, you will not need much compute power. Whereas if you use a camera, you will need much higher compute power and in-turn microcontrollers with higher clock speeds. 

### Power
Robots are typically mobile and use batteries as power source. So you need to be cautious on the power consumption. Some microcontrollers like the ESP32 have a ultra-low powered sleep mode which can come in handy for some long-duration low duty cycle applications. If you have such a application in mind, pay particular attention on the sleep mode and the current draw during those modes to see if the microcontroller can run without draining the batteries completely. 

### Easy of Use
Another major factor to consider, particularly if you are just getting started is the ease of use. There are plenty of programming environments. Choose the environment that fits your learning level and time constraints. For example, the Arduino environment is very simple and easy to use, whereas the Microchip PIC C/C++ environment is a little more complicated and more challenging. Once you’ve chosen a programming environment, make sure to read its instruction manual and check its community forums to find answers to common problems.

Both the development environment and the microcontroller need to be user-friendly to have a smooth experience. If you want to program the microcontroller using an IDE, you need to choose one that has the features that you need. You don’t want to buy a microcontroller only to find out that it doesn’t work with your IDE. On the other hand, if you want to program the microcontroller using a C/C++ compiler, you need to choose one that has features that you need. The IDE must have a debugger and other tools that help you to debug your code. If you are going to program the microcontroller using a C/C++ compiler, make sure that the microcontroller has the required I/O pins.

### Cost & Availability
After going through all the research and zeroing in on a microcontroller, you might notice that either the chosen microcontroller does not fit into your budget or it has a very long lead time or not available in your country. Particularly with the chip shortage and supply-chain disruptions due to the pandamic, it is getting much harder to source these chips. If you end up in this situation either due to cost or availability, you might have to go back to the drawing board to reconsider your requirements and see which ones you could compromise or modify. So it is always a good idea to have a couple of options all along so that if one of them become unavailable, you will have other options to pick. 

### Toolchain Support and Community
A toolchain is a collection of software development tools such as compilers, debuggers, simulators, etc. If your microcontroller does not have good toolchain support, you will have to debug the toolchain issues yourself or even worse, start setting up the toolchain from scratch. All major microcontrollers have good toolchain support and active community to back them up. Good toolchain support and an active community is important because you will have a ready pool of developers who can help you out and provide advice for the microcontroller. If your microcontroller has a active community, you can rely on those developers to help you. Also, microcontrollers can get outdated very fast. If you use a poorly supported toolchain, you may have to write your own drivers and libraries. If you want to use the latest technology and features, you should look for a microcontroller that has a good community and toolchain support. If the community and toolchain support is lacking, you might have to look for alternatives.

### Over-the-air (OTA) update 
OTA capability allows the software and firmware updates to be delivered to the device via wireless means. This method is useful for updating a microcontroller in the field. If you want to update your microcontroller remotely, you should look for a microcontroller with this capability. Sometimes you might rely on OTA update because the microcontroller programming ports are not exposed or in hard to access spots. In those situations, it is much helpful to perform an update over the air to update the code. This functionality is gaining popularity as more and more manufacturers are supporting this. 

## Conclusion
There are various factors you need to consider before choosing a microcontroller for your robotic project. In this article we touched upon some on the major factors you need to consider before you choose a microcontroller for your project. You need to know the following in order to choose the right microcontroller:

- Which features are required for your robot? For instance, if you want to use the microcontroller for controlling motors, you will have to look for a microcontroller with this capability.
- What are the advantages and disadvantages of each microcontroller? There are many options available. Some are better than others. You need to choose one which meets your needs.
- Is there a community and good toolchain support?
- Will the microcontroller have features that are necessary for your project?

In short, you should choose a microcontroller with the right features for your project. If the community and toolchain support is lacking, you might have to look for alternatives.