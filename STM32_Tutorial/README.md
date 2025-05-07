# Tracing OS events

In these tutorials, we will make intensive use a commercial tool to illustrate FreeRTOS mechanisms. This tool is Tracealyzer® from Percepio®.

You may take some time to visit Percepio® website:

- **Tracealyzer**Tracealyzer [main page](https://percepio.com/tracealyzer/)
- **Tracealyzer**Tracealyzer for FreeRTOS [user manual](https://percepio.com/getstarted/latest/html/freertos.html)
<p align="justify">
If you cannot get access to a **Tracealyzer** license (sorry for you), you may skip this tutorial and still profit from the several screenshots that you will find along these tutorials.
</p>
<p align="justify">
Otherwise, and if you want to be able to record and analyze what's happening in your FreeRTOS projects, you are highly encouraged to setup your project in order to get those OS traces. That's really helpful, and highly instructive.
</p>
The only thing you need to know at the moment is that tracing OS events requires two pieces of software:

- The **recorder** is running on your target MCU while OS is executing. It captures and records events at runtime in a memory buffer . It is provided as a set of source files (just like FreeRTOS) you need to include and configure in your MCU project. You can see it as a spying program working from the inside of the firmware.

- The **Tracealyzer** is a computer application which provides visual tools to help analyze OS behavior based on stored events. It uses the information gathered by the recorder and provide views into a user-friendly graphical interface.
There are two methods for transferring events stored in the MCU memory by the recorder to the **Tracealyzer** computer application:
- Using a memory dump from the debugger while program execution is suspended (**snapshot** mode)

- Using a real-time streaming while program executes (**streaming** mode)

This tutorial only covers the **snapshot** mode, which I find easier to setup for a start.

At time of writing, **Tracealyzer** version is 4.8.1. Again, I strongly encourage you to use the same version as me until you get enough understanding of the recorder integration.

## 1. Integrating the Recorder
### 1.1. Getting the Recorder source files into your project
You may get recorder source files from two locations:
- The FreeRTOS archive comes with various addons including the Percepio recorder source files. These are located in `FreeRTOS-Plus\Source\FreeRTOS-Plus-Trace\` folder.
- After you have downloaded and installed Percepio Tracealyzer on your computer, you'll find the recorder source files in the installation directory (`e.g. C:\Program Files\Percepio\Tracealyzer 4\FreeRTOS\TraceRecorder\`).
<p align="justify">
You can open the file \config\trcConfig.h in both distributions to see what you have. Be aware that if the recorder is prior to the FreeRTOS version, it might not work properly. Actually, the FreeRTOS release (v202212.01) comes with a FreeRTOS kernel in version  v10.5.1 and packages the Percepio recorder in version v4.6.0 that has apparently no support update since FreeRTOS v10.4.1.
</p>
<p align="justify">
On the other hand, the recorder that comes with Tracealyzer is in version 4.8.1 and displays support for FreeRTOS up to version 10.5.1. (even through the configuration for v10.5.1 refers to v10.4.0). Anyway, we will take that one!
</p>

<div align="center">
  <img src="Image\1.jpeg" alt="Centered Image" />
</div>

After you've located the recorder source files on your computer, create a new source folder `TraceRecorder` in your project structure and Copy/Paste source files.

Again, some cleanup is necessary:

- Delete the `/extras` folder

- In the `/streamports` folder, delete all folders except the /Jlink_RTT folder

- Remove all remaining files that are neither `source (.c)` nor `header (.h)`

By the end of the cleanup, your project structure should look like this. That's indeed a lot of new files, but don't worry, we will just deal with a few configuration headers.

<div align="center">
  <img src="Image\2.jpeg" alt="Centered Image" />
</div>

Next, add the TraceRecorder\include\ path to the build configuration and apply changes:

<div align="center">
  <img src="Image\3.jpeg" alt="Centered Image" />
</div>

### 1.2. Configuring the Recorder
Configuring the recorder requires some attention, especially because our target has very a limited amount of RAM (16kB). Most of this configuration takes place in headers located in the `/config` folder of the recorder source tree:

- trcConfig.h
- trcKernelPortConfig.h
- rcSnapshotConfig.h
- trcKernelPortSnapshotConfig.h

Let us visit each of these header and see what configuration we have to do.
#### Open trcConfig.h:
Comment out the error message and include our target header:
```
/******************************************************************************
 * Include of processor header file
 *
 * Here you may need to include the header file for your processor. This is
 * required at least for the ARM Cortex-M port, that uses the ARM CMSIS API.
 * Try that in case of build problems. Otherwise, remove the #error line below.
 *****************************************************************************/
// #error "Trace Recorder: Please include your processor's header file here and remove this line."
#include "stm32f0xx.h"
```
Specify the FreeRTOS hardware port we are using:
```
/**
 * @def TRC_CFG_HARDWARE_PORT
 * @brief Specify what hardware port to use (i.e., the "timestamping driver").
 *
 * All ARM Cortex-M MCUs are supported by "TRC_HARDWARE_PORT_ARM_Cortex_M".
 * This port uses the DWT cycle counter for Cortex-M3/M4/M7 devices, which is
 * available on most such devices. In case your device don't have DWT support,
 * you will get an error message opening the trace. In that case, you may
 * force the recorder to use SysTick timestamping instead, using this define:
 *
 * #define TRC_CFG_ARM_CM_USE_SYSTICK
 *
 * For ARM Cortex-M0/M0+ devices, SysTick mode is used automatically.
 *
 * See trcHardwarePort.h for available ports and information on how to
 * define your own port, if not already present.
 */
#define TRC_CFG_HARDWARE_PORT TRC_HARDWARE_PORT_ARM_Cortex_M
```
Then apply following tuning:
```
#define TRC_CFG_SCHEDULING_ONLY             0
#define TRC_CFG_INCLUDE_MEMMANG_EVENTS      0
#define TRC_CFG_INCLUDE_USER_EVENTS         0
#define TRC_CFG_INCLUDE_ISR_TRACING         0
#define TRC_CFG_INCLUDE_READY_EVENTS        1
#define TRC_CFG_INCLUDE_OSTICK_EVENTS       0

#define TRC_CFG_ENABLE_STACK_MONITOR        0
#define TRC_CFG_STACK_MONITOR_MAX_TASKS     10
#define TRC_CFG_STACK_MONITOR_MAX_REPORTS   1

#define TRC_CFG_CTRL_TASK_PRIORITY          1
#define TRC_CFG_CTRL_TASK_DELAY             10
#define TRC_CFG_CTRL_TASK_STACK_SIZE        1024

#define TRC_CFG_RECORDER_BUFFER_ALLOCATION  TRC_RECORDER_BUFFER_ALLOCATION_STATIC
#define TRC_CFG_MAX_ISR_NESTING             8

#define TRC_CFG_ISR_TAILCHAINING_THRESHOLD  0
#define TRC_CFG_RECORDER_DATA_INIT          1

#define TRC_CFG_RECORDER_DATA_ATTRIBUTE 

#define TRC_CFG_USE_TRACE_ASSERT            0
```
