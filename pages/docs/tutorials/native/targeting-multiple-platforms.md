---
type: tutorial
layout: tutorial
title:  "Targeting Multiple Platforms"
description: "Compiling with Kotlin/Native for different platforms"
authors: Eugene Petrenko
date: 2018-07-10
showAuthorInfo: false
issue: EVAN-5121
---

With Kotlin/Native, you can compile a native binary for 
the following platforms, including (as of Kotlin/Native v0.8): 
- Windows (x86_64)
- Linux (x86_64, arm32, MIPS, MIPS little endian)
- MacOS (x86_64)
- iOS (arm32, arm64, x64)
- Android (arm32, arm64)
- STM32
- WebAssembly (wasm32)

In this tutorial, we will see how to

* [Specify a Target Platform](#specifying-a-target-platform)
* [Build for a Specific Platform](#building-for-a-specific-platform)
* [Build a Console Utility for several Operating Systems](#building-a-console-utility)

We need to have a Kotlin/Native compiler on our machines. 
You may have a look at the
[A Basic Kotlin/Native Application](basic-kotlin-native-app.html#obtaining-the-compiler)
tutorial for more information on performing this step.
Let's assume we have a console, where `kotlinc` (or the older `konanc`) command is available. 

## Specifying a Target Platform

The list of supported target platforms of `kotlinc` depends 
on the operating system where you run it. We may list them via 

```bash
kotlinc -list_targets
```

command. For example, on macOS X with Kotlin/Native v0.8:
```
> kotlinc -list_targets
macos_x64:                    (default) macbook, macos, imac
ios_arm32:                              iphone32
ios_arm64:                              iphone, ipad, ios
ios_x64:                                iphone_sim
android_arm32:
android_arm64:
wasm32:
zephyr_stm32f4_disco:
```

For Linux and Windows Subsystem for Linux (WSL) with Kotlin/Native v0.8 we have:

```
> kotlinc -list_targets
linux_x64:                    (default) linux
linux_arm32_hfp:                        raspberrypi
linux_mips32:
linux_mipsel32:
android_arm32:
android_arm64:
wasm32:
zephyr_stm32f4_disco:
```

On Windows with Kotlin/Native v0.8 it shows:
```
> kotlinc -list_targets
mingw_x64:                    (default) mingw
wasm32:
zephyr_stm32f4_disco:
```

The set of targets for the Kotlin/Native compiler depends on the host operating system.
We may specify a target explicitly with a `-target <name>` argument. The default target 
is highlighted with `(default)` and used if no `-target` argument was 
specified.

## Building for a Specific Platform

Let's create a sample Kotlin/Native program and save it as `main.kt`. 
You may want to have a look at the tutorial
[A Basic Kotlin/Native Application](basic-kotlin-native-app.html#creating-hello-kotlin) for more details.

<div class="sample" markdown="1" theme="idea">
```kotlin
fun main(args: Array<String>) {
  println("Hello Kotlin/Native!")
}
```
</div>

We use the `-target` argument of the `kotlinc` to specify the platform. It is also 
helpful to use `-output` to explicitly instruct
the compiler on where to create the compiled binary, for example, to build for the iOS emulator on macOS we use:

```bash
    kotlinc -target ios_x64 -output bin/ios_x64 main.kt
``` 

Native executable `bin/ios_x64.kexe` will be produced. We check that with the `file` command 
```
> file bin/ios_x64.kexe
bin/ios_x64.kexe: Mach-O 64-bit executable x86_64
```

We use the command 
```bash
    kotlinc -target ios_arm64 -output bin/ios_arm64 main.kt
```
to create a binary for an iPhone supporting arm64. 

## Building a Console Utility

We saw above that the set of supported target platforms depends on the host operating system.
Running the compiler on Windows, macOS, and Linux is required to create a console application 
for those operating systems. On each of the operating systems we call the compiler:

```bash
kotlinc main.kt -output bin/theTool
```

The `-target` parameter is optional. If it is not included, the binary will be produced for the 
system, where the compiler is executed. It matches the behavior of other native compilers.
A [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration)
may be used to automate and simplify the process. 

## Next Steps

Build tools help to deal with compiler arguments. You might want to consider 
using [Gradle build system](gradle-for-kotlin-native.html) for your project. 
Gradle with the Kotlin/Native plugin simplifies the setup for every operating system, downloads and runs 
the Kotlin/Native compiler for you. It will be even more pleasant for you
