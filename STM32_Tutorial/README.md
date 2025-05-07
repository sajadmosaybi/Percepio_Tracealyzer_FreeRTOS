# Tracing OS events
In these tutorials, we will make intensive use a commercial tool to illustrate FreeRTOS mechanisms. This tool is Tracealyzer® from Percepio®.

You may take some time to visit Percepio® website:

- **Tracealyzer**Tracealyzer [main page](https://percepio.com/tracealyzer/)
- **Tracealyzer**Tracealyzer for FreeRTOS [user manual](https://percepio.com/getstarted/latest/html/freertos.html)
If you cannot get access to a **Tracealyzer** license (sorry for you), you may skip this tutorial and still profit from the several screenshots that you will find along these tutorials.

Otherwise, and if you want to be able to record and analyze what's happening in your FreeRTOS projects, you are highly encouraged to setup your project in order to get those OS traces. That's really helpful, and highly instructive.

The only thing you need to know at the moment is that tracing OS events requires two pieces of software:

- The **recorder** is running on your target MCU while OS is executing. It captures and records events at runtime in a memory buffer . It is provided as a set of source files (just like FreeRTOS) you need to include and configure in your MCU project. You can see it as a spying program working from the inside of the firmware.

- The **Tracealyzer** is a computer application which provides visual tools to help analyze OS behavior based on stored events. It uses the information gathered by the recorder and provide views into a user-friendly graphical interface.
There are two methods for transferring events stored in the MCU memory by the recorder to the **Tracealyzer** computer application:

- Using a memory dump from the debugger while program execution is suspended (**snapshot** mode)

- Using a real-time streaming while program executes (**streaming** mode)

This tutorial only covers the **snapshot** mode, which I find easier to setup for a start.

At time of writing, **Tracealyzer** version is 4.8.1. Again, I strongly encourage you to use the same version as me until you get enough understanding of the recorder integration. You can get **Tracealyzer** installers from the Percepio® website: https://percepio.com/tracealyzer/update.
