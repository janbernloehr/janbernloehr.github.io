---
layout: post
title : "Compiling UDF with ANSYS 16 and Visual Studio 2015"
date : 07.06.2016 01:59:00
tags: [ansys, visual-studio, udf]
---
{% include JB/setup %}

To compile User Defined Functions (UDF) with ANSYS you need to install a C++ compiler. ANSYS recommends Visual C++ which is freely available in form of [Visual Studio Community](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx).

In this article I will only consider the case of x64 ANSYS.

## 1. Update udf.bat

Go to `C:\Program Files\ANSYS Inc\v162\fluent\ntbin\win64` and back up the file `udf.bat`. We need to modify this file to add support for Visual C++ 2015. To this end, add **below** `set MSVC_VERSION=0` the following lines

```
echo trying to find MS C compiler, version 140....

set MSVC_DEFAULT=%ProgramFiles(x86)%\Microsoft Visual Studio 14.0
if exist "%MSVC_DEFAULT%\VC\vcvarsall.bat" set MSVC=%MSVC_DEFAULT%
if not "%MSVC%" == "" goto msvc_env140
```

Moreover, add **above** `:msvc_env71` the following lines

```
:msvc_env140
set MSVC_VERSION=140
call "%MSVC%\VC\vcvarsall.bat" x64
goto ms_c_end
```

Save the file `udf.bat` and you are good to go.

## 2. Compile your UDF

*   Make sure your code, header, mesh and fluent files are contained in a single folder (project folder).
*   Open ANSYS fluent and select the project folder as the current working directory.
*   In ANSYS' main window select Define -> User Defined Function -> Compile

## X. Hint

Make sure in your UDF file's `DEFINE` statements are not spaces between brackets.

DONT

`DEFINE_PROFILE (current_dens,t,i)`

DO

`DEFINE_PROFILE(current_dens,t,i)`

Additional spaces may lead to erratic behavior of the makefile script so that defined SOURCES, PROFILES, or DIFFUSIVITIES do not show in Fluent.
