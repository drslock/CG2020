## Introduction and Orientation
### <a href='https://web.microsoftstream.com/channel/b84051cb-1dba-4cb4-a271-60cc6635f92f' target='_blank'> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction


The aim of this short introductory workbook is to ease everybody gently into the first week of term. The main objective is to make sure that you are able to build and run the code template that you will need to complete this unit. This may take some time and perseverance to achieve (depending on the computer that you intend to use for this unit). It is therefore **ESSENTIAL** that you achieve everything in this workbook during the first week of term. If you are unable to build and run applications, you will find yourself falling behind on this unit.

If you didn't make it to the live briefing session earlier in the week, make sure you have watched the "Weekly Briefing" video above before commencing the remaining tasks in this week's workbook !  


# 
### Task 2: Language and Libraries


The implementation language for this unit will be C/C++. Everybody will have their own preferred language for writing code, however C/C++ is very much the standard for low-level graphics rendering applications. If we didn't use this as the primary language for this unit, potential employers would think it very unusual.

The aim of this unit is to develop a fundamental understanding of graphics rendering and as such, we will not be making use of any existing rendering frameworks (but rather, building our own !) That said, the practical exercises will make use of the following two external libraries:
- [GLM](http://glm.g-truc.net) for general purpose mathematical operations (mostly for manipulating vectors and matrices)
- [SDL2](http://www.libsdl.org) for drawing to the screen (mostly setting the colour of individual pixels !)

Remember: The aim of this unit is to learn to build things from the ground-up, so don't use anything other than SDL2 and GLM when implementing the practical exercises.  

In order to make things a little tidier, we have provided a "wrapper" object around SDL2 called _DrawingWindow_ . This wrapper initialises SDL, opens up a window for drawing to the screen and then provides various methods for drawing and interacting with the user. The _DrawingWindow_ object has the following methods:  

- DrawingWindow: A constructor for the drawing window that takes 3 parameters:
    - Horizontal resolution for the window (integer width)
    - Vertical resolution for the window (integer height)
    - Whether or not to show the window in full-screen mode (boolean)
- setPixelColour: sets an individual pixel (indicated by x and y coordinates) to a specified colour (a packed ARGB value)
- getPixelColour: returns the colour of the pixel indicated by x and y coordinates
- renderFrame: draws all pixels to the screen (until you call this, all pixel changes are just in a memory buffer)
- savePPM: saves the currently rendered content of the window to a .ppm image file
- saveBMP: saves the currently rendered content of the window to a .bmp image file
- clearPixels: clears all pixel colours (from the whole window)
- pollForInputEvents: checks to see if there are any events waiting to be processed (returns true if there are !)

Examples of how to call the methods of the DrawingWindow class are included in the template project introduced later in this workbook. 
  


# 
### Task 3: Using Your Own Machine


We have tried to use a programming environment that is agnostic to the underlying architecture and operating system (so it should be _fairly_ straight-forward to get running on most machines !) We will test and mark assignment on the lab machines so you should not use any platform-specific features.  If you use an IDE (Visual Studio, Eclipse, XCode etc) make sure that your project compiles without it (i.e. by providing a Make file that works on the lab machines).

Installation of SDL2 can sometimes be non-trivial, due to the fact that it must link with native graphics libraries. For this reason, the recommended way to install it is using your operating system's package manager (e.g. apt, rpm, yum, brew, ports etc). The name of the required package will vary depending on your package manager. Here are some likely candidates for a range of different package managers:
```
apt install libsdl2-dev
rpm -i SDL2-devel
yum install SDL2-devel
brew install sdl2
port install libsdl2
```

If you can't get your package manager to install SDL2 (or if you don't have a package manager at all !) then you will have to install the **development libraries** manually from: [https://www.libsdl.org/download-2.0.php](https://www.libsdl.org/download-2.0.php). This _may_ involve manually copying libraries/headers and potentially setting up environment variables - there are plenty of guides for specific platforms online. Note that on some platforms you might have to alter the `include` statement in the DrawingWindow.h file from `"SDL.h"` to `"SDL2/SDL.h"`

The code template (that we will introduce later in this workbook) comes bundled with the GLM library built-in, so there is no need for you to install this yourselves !
  


**Hints & Tips:**  
If you are having difficulties installing SDL2 on your existing operating system, we have created a Vagrant box <a href="Vagrantfile" target="_blank">configuration file</a> that can be used to bring up a virtual machine with all of the required tools and libraries installed. See the comments at the top of the file for instructions on how to get started with Vagrant.

Alternatively (if you are confident to do so) it might be worth considering a "fresh" install of your preferred linux distribution. It should be relatively straight-forward to install SDL2 on a clean linux install. You might like to consider dual booting your computer (if you are brave enough !) or alternatively, use a virtual machine (such as VirtualBox) and install linux within.

Note: If using a virtual machine, you should avoid using display scaling (i.e. use VirtualBox in 100% "Unscaled Output" mode) otherwise you may experience a reduction in window manager performance.  


# 
### Task 4: Using the Lab Machines


As an alternative to getting SDL2 installed on your own computer, you might choose to compile and run your code remotely on a lab machine. In order to gain remote access (including remote viewing of graphics) you will need to use _X2Go_. Full instructions for installing and running the _X2Go_ client can be found in <a href="https://uob.sharepoint.com/sites/itservices/SitePages/fits-engineering-linux-x2go.aspx" target="_blank">this how-to guide</a>. It is worth noting however that the quality of your experience using this approach to development will vary depending on the quality of your internet connection.


   


**Hints & Tips:**  
For help resolving problems with remote access to the lab machines, check out the
<a href="https://uob.sharepoint.com/sites/itservices/SitePages/contacts.aspx" target="_blank">IT services help page</a>
in particular the rapid response teaching support helpline !


  


# 
### Task 5: The Template Project


In order to help get you started, we have provided a sample project called <a href="../../RedNoise/" target="_blank">RedNoise</a>. This project provides the structure that you will need for each weekly workbook, as well as illustrating the use of the _DrawingWindow_ methods. You should use this template as a starting point for each practical exercise. Do be sure to change the name of the project (you don't want to have a bunch of projects all called _RedNoise_ !).

In order to help you compile, link and run your code, a Makefile has been provided as part of the template project. Be sure you update the `PROJECT_NAME` on the first line of the Makefile to match the name of your project/main class each week. This Makefile has multiple build rules for different purposes:

- debug: A development build rule that will compile and link your project for use with a debugger (gdb)
- diagnostic: A development build rule that includes extra memory checking and diagnosis flags.  
Note: this rule requires the [Address Sanitizer](https://clang.llvm.org/docs/AddressSanitizer.html) library to be installed (which comes built-in with some C++ compilers)
- speedy: A build rule that will result in a high performance executable (to make interaction testing quicker)
- production: A build rule to make an executable without debug hooks (for release or distribution)

Just typing `make` on its own will build the _debug_ rule and run the resultant executable.  

When you successfully build and run the _RedNoise_ project you should see a window that looks like the following...  


![](05%20The%20Template%20Project/images/red-noise.jpg)

**Hints & Tips:**  
Depending on the compiler that you have installed, you may need to edit the _Makefile_ in order to change the compiler used to build the project (currently the project uses `clang++` but you may need to change this to `g++` or `cl`)

A _CMake_ build file <a href="../../RedNoise/CMakeLists.txt" target="_blank">CMakeLists.txt</a> is also included in the _RedNoise_ project (in case you prefer using _CMake_ rather than _Make_). Details of how to use _CMake_ are included as comments in the `CMakeLists.txt` file. This configuration file will also allow you to import and automatically configure the project in C++ IDEs such as _CLion_.

On some platforms you may need to include `<cstring>` in the DrawingWindow class (if the compiler complains that it can't find `memset`).

The cleanest way to quit an SDL application is by pressing the ESC key ! If you have a window that you can't close (which does happen from time to time), you may need to kill off the process manually using Task Manager, Activity Monitor or the kill/killall command.  


# 
### Task 6: Understanding the Template


It is useful to spend a little time getting to know the structure of the <a href="../../RedNoise/src/RedNoise.cpp" target="_blank">template project code</a> - we are going to be using it a lot over the next few weeks. The main function in the code contains a loop which:

1. Polls the event queue for incoming mouse and keyboard events
2. Updates the position of any moving elements of the scene
3. Draws the scene to the screen memory buffer
4. Renders the memory buffer onto the SDL window

Drawing the scene onto the screen buffer basically involves looping through a pixel at a time, deciding what colour it should be, packing that colour into an ARGB integer and setting the relevant pixel to that colour. The _RedNoise_ example just creates a random pattern of red pixels of varying brightness. This is a good starting point to make sure that the code runs - in later weeks we are going to do more sophisticated things with the pixels !

You will note from the `handleEvent` method that it is possible to respond to both mouse and keyboard events. In the _RedNoise_ example we simply print out messages to the console (by piping strings to the `cout` object). In later weeks we are going to use these event handlers to allow the user to navigate around a scene and switch between different rendering modes.
  


# 
### Progress Logbook

It is important to be able to monitor the progress of students as they progress through these workbooks.
This is to ensure that everyone is keeping up to date with the work and to identify students who are struggling.
For these reasons, it is **ESSENTIAL** that you keep a logbook of progress that you are making.
To minimise the overhead in keeping this logbook, you are only required to capture a single screenshot each week.
Try to capture something that summarises the progress that you have made and upload this onto the
[logbook submission area on blackboard](https://www.ole.bris.ac.uk/webapps/assignment/uploadAssignment?content_id=_4837989_1&course_id=_240795_1&group_id=&mode=cpview).
(Note: you might have to log into blackboard _first_ before your click on this link !).
It is important that **EVERYBODY** uploads an image (so we can be sure that everyone is engaging with the unit).
If you don't keep your logbook up to date then you'll find us getting in touch to see if everything is OK !