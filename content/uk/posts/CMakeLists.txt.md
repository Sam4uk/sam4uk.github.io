---
title: CMakeLists.txt
date: 2024-06-02
description: CMakeLists.txt
draft: false
# weight: 2
# pinned: true
# tags:
# - C
# - C++
image: https://www.freeiconspng.com/uploads/timer-icon-31.png
---


indexnext |previous | CMake » 
3.28.1
 Documentation » CMake Tutorial » Step 1: A Basic Starting Point
Step 1: A Basic Starting Point
Where do I start with CMake? This step will provide an introduction to some of CMake's basic syntax, commands, and variables. As these concepts are introduced, we will work through three exercises and create a simple CMake project.

Each exercise in this step will start with some background information. Then, a goal and list of helpful resources are provided. Each file in the Files to Edit section is in the Step1 directory and contains one or more TODO comments. Each TODO represents a line or two of code to change or add. The TODO s are intended to be completed in numerical order, first complete TODO 1 then TODO 2, etc. The Getting Started section will give some helpful hints and guide you through the exercise. Then the Build and Run section will walk step-by-step through how to build and test the exercise. Finally, at the end of each exercise the intended solution is discussed.

Also note that each step in the tutorial builds on the next. So, for example, the starting code for Step2 is the complete solution to Step1.

Exercise 1 - Building a Basic Project
The most basic CMake project is an executable built from a single source code file. For simple projects like this, a CMakeLists.txt file with three commands is all that is required.

Note: Although upper, lower and mixed case commands are supported by CMake, lower case commands are preferred and will be used throughout the tutorial.

Any project's top most CMakeLists.txt must start by specifying a minimum CMake version using the cmake_minimum_required() command. This establishes policy settings and ensures that the following CMake functions are run with a compatible version of CMake.

To start a project, we use the project() command to set the project name. This call is required with every project and should be called soon after cmake_minimum_required(). As we will see later, this command can also be used to specify other project level information such as the language or version number.

Finally, the add_executable() command tells CMake to create an executable using the specified source code files.

Goal
Understand how to create a simple CMake project.

Helpful Resources
add_executable()

cmake_minimum_required()

project()

Files to Edit
CMakeLists.txt

Getting Started
The source code for tutorial.cxx is provided in the Help/guide/tutorial/Step1 directory and can be used to compute the square root of a number. This file does not need to be edited in this step.

In the same directory is a CMakeLists.txt file which you will complete. Start with TODO 1 and work through TODO 3.

Build and Run
Once TODO 1 through TODO 3 have been completed, we are ready to build and run our project! First, run the cmake executable or the cmake-gui to configure the project and then build it with your chosen build tool.

For example, from the command line we could navigate to the Help/guide/tutorial directory of the CMake source code tree and create a build directory:

mkdir Step1_build
Next, navigate to that build directory and run cmake to configure the project and generate a native build system:

cd Step1_build
cmake ../Step1
Then call that build system to actually compile/link the project:

cmake --build .
For multi-config generators (e.g. Visual Studio), first navigate to the appropriate subdirectory, for example:

cd Debug
Finally, try to use the newly built Tutorial:

Tutorial 4294967296
Tutorial 10
Tutorial
Note: Depending on the shell, the correct syntax may be Tutorial, ./Tutorial or .\Tutorial. For simplicity, the exercises will use Tutorial throughout.

Solution
As mentioned above, a three line CMakeLists.txt is all that we need to get up and running. The first line is to use cmake_minimum_required() to set the CMake version as follows:

TODO 1: Click to show/hide answer
The next step to make a basic project is to use the project() command as follows to set the project name:

TODO 2: Click to show/hide answer
The last command to call for a basic project is add_executable(). We call it as follows:

TODO 3: Click to show/hide answer
Exercise 2 - Specifying the C++ Standard
CMake has some special variables that are either created behind the scenes or have meaning to CMake when set by project code. Many of these variables start with CMAKE_. Avoid this naming convention when creating variables for your projects. Two of these special user settable variables are CMAKE_CXX_STANDARD and CMAKE_CXX_STANDARD_REQUIRED. These may be used together to specify the C++ standard needed to build the project.

Goal
Add a feature that requires C++11.

Helpful Resources
CMAKE_CXX_STANDARD

CMAKE_CXX_STANDARD_REQUIRED

set()

Files to Edit
CMakeLists.txt

tutorial.cxx

Getting Started
Continue editing files in the Step1 directory. Start with TODO 4 and complete through TODO 6.

First, edit tutorial.cxx by adding a feature that requires C++11. Then update CMakeLists.txt to require C++11.

Build and Run
Let's build our project again. Since we already created a build directory and ran CMake for Exercise 1, we can skip to the build step:

cd Step1_build
cmake --build .
Now we can try to use the newly built Tutorial with same commands as before:

Tutorial 4294967296
Tutorial 10
Tutorial
Solution
We start by adding some C++11 features to our project by replacing atof with std::stod in tutorial.cxx. This looks like the following:

TODO 4: Click to show/hide answer
To complete TODO 5, simply remove #include <cstdlib>.

We will need to explicitly state in the CMake code that it should use the correct flags. One way to enable support for a specific C++ standard in CMake is by using the CMAKE_CXX_STANDARD variable. For this tutorial, set the CMAKE_CXX_STANDARD variable in the CMakeLists.txt file to 11 and CMAKE_CXX_STANDARD_REQUIRED to True. Make sure to add the CMAKE_CXX_STANDARD declarations above the call to add_executable().

TODO 6: Click to show/hide answer
Exercise 3 - Adding a Version Number and Configured Header File
Sometimes it may be useful to have a variable that is defined in your CMakelists.txt file also be available in your source code. In this case, we would like to print the project version.

One way to accomplish this is by using a configured header file. We create an input file with one or more variables to replace. These variables have special syntax which looks like @VAR@. Then, we use the configure_file() command to copy the input file to a given output file and replace these variables with the current value of VAR in the CMakelists.txt file.

While we could edit the version directly in the source code, using this feature is preferred since it creates a single source of truth and avoids duplication.

Goal
Define and report the project's version number.

Helpful Resources
<PROJECT-NAME>_VERSION_MAJOR

<PROJECT-NAME>_VERSION_MINOR

configure_file()

target_include_directories()

Files to Edit
CMakeLists.txt

tutorial.cxx

Getting Started
Continue to edit files from Step1. Start on TODO 7 and complete through TODO 12. In this exercise, we start by adding a project version number in CMakeLists.txt. In that same file, use configure_file() to copy a given input file to an output file and substitute some variable values in the input file content.

Next, create an input header file TutorialConfig.h.in defining version numbers which will accept variables passed from configure_file().

Finally, update tutorial.cxx to print out its version number.

Build and Run
Let's build our project again. As before, we already created a build directory and ran CMake so we can skip to the build step:

cd Step1_build
cmake --build .
Verify that the version number is now reported when running the executable without any arguments.

Solution
In this exercise, we improve our executable by printing a version number. While we could do this exclusively in the source code, using CMakeLists.txt lets us maintain a single source of data for the version number.

First, we modify the CMakeLists.txt file to use the project() command to set both the project name and version number. When the project() command is called, CMake defines Tutorial_VERSION_MAJOR and Tutorial_VERSION_MINOR behind the scenes.

TODO 7: Click to show/hide answer
Then we used configure_file() to copy the input file with the specified CMake variables replaced:

TODO 8: Click to show/hide answer
Since the configured file will be written into the project binary directory, we must add that directory to the list of paths to search for include files.

Note: Throughout this tutorial, we will refer to the project build and the project binary directory interchangeably. These are the same and are not meant to refer to a bin/ directory.

We used target_include_directories() to specify where the executable target should look for include files.

TODO 9: Click to show/hide answer
TutorialConfig.h.in is the input header file to be configured. When configure_file() is called from our CMakeLists.txt, the values for @Tutorial_VERSION_MAJOR@ and @Tutorial_VERSION_MINOR@ will be replaced with the corresponding version numbers from the project in TutorialConfig.h.

TODO 10: Click to show/hide answer
Next, we need to modify tutorial.cxx to include the configured header file, TutorialConfig.h.

TODO 11: Click to show/hide answer
Finally, we print out the executable name and version number by updating tutorial.cxx as follows:

TODO 12: Click to show/hide answer
indexnext |previous | CMake » 
3.28.1
 Documentation » CMake Tutorial » Step 1: A Basic Starting Point
© Copyright 2000-2023 Kitware, Inc. and Contributors. Created using Sphinx 6.2.1.