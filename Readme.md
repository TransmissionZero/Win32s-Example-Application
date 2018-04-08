# Win32s Example Application

## Table of Contents

- [Introduction](#introduction)
- [Building the Application](#building-the-application)
- [Visual C++ and Win32s Compatibility](#visual-c-crts-and-win32s-compatibility)
- [Display Quirks with Visual C++ 2.x](#display-quirks-with-visual-c-2x)
- [Terms of Use](#terms-of-use)
- [Problems?](#problems)
- [Changelog](#changelog)

## Introduction

This application is an example Windows GUI application, written to demonstrate how to target
[Win32s](https://en.wikipedia.org/wiki/Win32s). It is based on the
[MSVC Win32 Application](https://github.com/TransmissionZero/MSVC-Win32-Application), but is stripped down to ensure it
only uses Win32 API which are present in Win32s and Windows NT 3.1. The provided project file is for Visual C++ 2.x. You
can [downloaded Visual C++ 2.0](https://my.visualstudio.com/Downloads?pid=141) if you have a Visual Studio Subscription.

If you are not trying to produce a Win32s application, you are advised to use the
[MSVC Win32 Application](https://github.com/TransmissionZero/MSVC-Win32-Application) instead. That makes use of more
modern Windows features such a visual styles and Unicode, and can be compiled as a 64 bit application without source
code modifications.

## Building The Application

To build the application using the IDE, open the Makefile in Visual C++ 2.x, choose the debug or release build from the
drop-down menu, and use the "build" button.

To build the application from the command prompt, ensure the Visual C++ "bin" directory is added to the %PATH%
environment variable, "include" directory is added to the %INCLUDE% environment variable, and "lib" directory is added
to the %LIB% environment variable. Once the environment is configured, type one of the following (as appropriate for a
debug or release build):

```
nmake /f W32sApp.mak "CFG=Win32 Debug"
nmake /f W32sApp.mak "CFG=Win32 Release"
```

Building from the command line works with many other versions of Visual C++ / Visual Studio too, but you may get
warnings about unknown or deprecated command line options, and you won't get a Win32s compatible executable unless you
use a version of Visual C++ which supports Win32s.

It should also be possible to build the application using any C or C++ compiler which supports targeting Win32s. You
will of course need to set the projects up for yourself if you do that. Open Watcom does claim to support Win32s, but I
haven't had any success in getting it to produce an application which works with Win32s.

## Visual C++, CRTs, and Win32s Compatibility

The only versions of Visual C++ to officially support Win32s are versions 1.0 (32 bit) to 4.1. Versions 4.2 onwards do
not support Win32s. Additionally, there are a number of different CRT DLLs, and you need to be careful to link against
and distribute the correct DLLs with your application:

- MSVCRT10.DLL: This is the Visual C++ 1.0 (32 bit) CRT DLL. It does not support Win32s.
- MSVCRT20.DLL: This is the Visual C++ 2.x CRT DLL. There are separate Win32s and Windows NT / 9x versions. The Win32s
  version does not work on NT / 9x, and the NT / 9x version does not work on Win32s, so you need to be careful to deploy
  the correct version!
- MSVCRT40.DLL: This is the Visual C++ 4.0 and 4.1 CRT DLL (but not Visual C++ 4.2!). As with MSVCRT20.DLL, you must be
  careful as there are separate Win32s and NT / 9x versions.
- CRTDLL.DLL: This is not a redistributable DLL, but is present in Win32s and all x86 and x64 versions of Windows.
  Applications can link against it by using the "CRTDLL.LIB" import library which can be found in the Windows NT 3.x
  SDK.

Therefore it is recommended you use the Windows NT 3.x SDK if you have it as it will save you a lot of deployment
trouble. If you are using Visual C++ (32 bit), then this SDK is your only option for producing Win32s applications.
Unfortunately, Windows NT 3.x SDKs are not available through Visual Studio subscriptions.

Although Visual C++ 4.2 and 5.0 don't officially support Win32s, you can still produce Win32s executables if you link
against `crtdll.lib` using the Windows NT 3.x SDK. In the case of Visual C++ 5.0, it is necessary to add the `/FIXED:NO`
linker option, as this version of the linker doesn't generate relocations by default. Visual C++ 6.0 onwards can't
produce Win32s compatible applications without a lot of fiddling with linker options, and that results in very large
executables due to disabling numerous linker optimisations. Trying to build a Win32s executable with Visual C++ 6.0 or
later probably isn't worth the effort.

## Display Quirks with Visual C++ 2.x

An interesting point is that Visual C++ 2.x produces executables which target a minimum Windows version of 3.10
(specified in the executable's PE header), whereas Visual C++ 4.x targets a minimum version of 4.00 (i.e. Windows NT 4
and Windows 95). This is mostly of little significance, but Windows 3.x applications look slightly different from
Windows 4.x applications in Windows NT 3.5x and later. The dialogs are larger, have a white background, and have bold
text. Windows looks at the executable's PE header and applies the "DS_3DLOOK" style to dialogs automatically if the
susbsystem version is 4.00 or later, regardless of whether the dialog template specifies this style. You can see this
for yourself by running `editbin /SUBSYSTEM:WINDOWS,3.10` or `editbin /SUBSYSTEM:WINDOWS,4.00` on a copy of the
executable and comparing how the dialogs look.

## Terms of Use

Refer to "License.txt" for terms of use.

## Problems?

If you have any problems or questions, please ensure you have read this readme file. If you are still having trouble,
you can [get in contact](http://www.transmissionzero.co.uk/contact/).

## Changelog

1. 2017-05-28: Version 1.0
  - Initial release.

Transmission Zero
2017-05-28
