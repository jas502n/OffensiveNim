osproc
==

`./1.4.0/nim/lib/pure/osproc.nim`


```
proc execProcess*(command: string, workingDir: string = "",
proc execCmd*(command: string): int {.rtl, extern: "nosp$1",
proc startProcess*(command: string, workingDir: string = "",
proc close*(p: Process) {.rtl, extern: "nosp$1", tags: [WriteIOEffect].}
proc suspend*(p: Process) {.rtl, extern: "nosp$1", tags: [].}
proc resume*(p: Process) {.rtl, extern: "nosp$1", tags: [].}
proc terminate*(p: Process) {.rtl, extern: "nosp$1", tags: [].}
proc kill*(p: Process) {.rtl, extern: "nosp$1", tags: [].}
proc running*(p: Process): bool {.rtl, extern: "nosp$1", tags: [].}
proc processID*(p: Process): int {.rtl, extern: "nosp$1".} =
proc waitForExit*(p: Process, timeout: int = -1): int {.rtl,
proc peekExitCode*(p: Process): int {.rtl, extern: "nosp$1", tags: [].}
proc inputStream*(p: Process): Stream {.rtl, extern: "nosp$1", tags: [].}
proc outputStream*(p: Process): Stream {.rtl, extern: "nosp$1", tags: [].}
proc errorStream*(p: Process): Stream {.rtl, extern: "nosp$1", tags: [].}
proc peekableOutputStream*(p: Process): Stream {.rtl, extern: "nosp$1", tags: [], since: (1, 3).}
proc peekableErrorStream*(p: Process): Stream {.rtl, extern: "nosp$1", tags: [], since: (1, 3).}
proc inputHandle*(p: Process): FileHandle {.rtl, extern: "nosp$1",
proc outputHandle*(p: Process): FileHandle {.rtl, extern: "nosp$1",
proc errorHandle*(p: Process): FileHandle {.rtl, extern: "nosp$1",
proc countProcessors*(): int {.rtl, extern: "nosp$1".} =
proc execProcesses*(cmds: openArray[string],
proc readLines*(p: Process): (seq[string], int) {.since: (1, 3).} =
proc execProcess(command: string, workingDir: string = "",
proc hsClose(s: Stream) =
proc hsAtEnd(s: Stream): bool = return FileHandleStream(s).atTheEnd
proc hsReadData(s: Stream, buffer: pointer, bufLen: int): int =
proc hsWriteData(s: Stream, buffer: pointer, bufLen: int) =
proc newFileHandleStream(handle: Handle): owned FileHandleStream =
proc buildCommandLine(a: string, args: openArray[string]): string =
proc buildEnv(env: StringTableRef): tuple[str: cstring, len: int] =
proc open_osfhandle(osh: Handle, mode: int): int {.
proc myDup(h: Handle; inherit: WINBOOL = 1): Handle =
proc createAllPipeHandles(si: var STARTUPINFO;
proc createPipeHandles(rdHandle, wrHandle: var Handle) =
proc fileClose(h: Handle) {.inline.} =
proc startProcess(command: string, workingDir: string = "",
proc close(p: Process) =
proc suspend(p: Process) =
proc resume(p: Process) =
proc running(p: Process): bool =
proc terminate(p: Process) =
proc kill(p: Process) =
proc waitForExit(p: Process, timeout: int = -1): int =
proc peekExitCode(p: Process): int =
proc inputStream(p: Process): Stream =
proc outputStream(p: Process): Stream =
proc errorStream(p: Process): Stream =
proc peekableOutputStream(p: Process): Stream =
proc peekableErrorStream(p: Process): Stream =
proc execCmd(command: string): int =
proc select(readfds: var seq[Process], timeout = 500): int =
proc hasData*(p: Process): bool =
proc isExitStatus(status: cint): bool =
proc envToCStringArray(t: StringTableRef): cstringArray =
proc envToCStringArray(): cstringArray =
proc startProcessAuxSpawn(data: StartProcessData): Pid {.
proc startProcessAuxFork(data: StartProcessData): Pid {.
proc startProcessAfterFork(data: ptr StartProcessData) {.
proc startProcess(command: string, workingDir: string = "",
proc startProcessAuxSpawn(data: StartProcessData): Pid =
proc startProcessAuxFork(data: StartProcessData): Pid =
proc startProcessFail(data: ptr StartProcessData) =
proc startProcessAfterFork(data: ptr StartProcessData) =
proc close(p: Process) =
proc suspend(p: Process) =
proc resume(p: Process) =
proc running(p: Process): bool =
proc terminate(p: Process) =
proc kill(p: Process) =
proc waitForExit(p: Process, timeout: int = -1): int =
proc waitForObjects(infos: ptr ObjectWaitInfo, numInfos: cint, flags: uint32,
proc waitForExit(p: Process, timeout: int = -1): int =
proc waitForExit(p: Process, timeout: int = -1): int =
proc peekExitCode(p: Process): int =
proc createStream(handle: var FileHandle,
proc inputStream(p: Process): Stream =
proc outputStream(p: Process): Stream =
proc errorStream(p: Process): Stream =
proc peekableOutputStream(p: Process): Stream =
proc peekableErrorStream(p: Process): Stream =
proc csystem(cmd: cstring): cint {.nodecl, importc: "system",
proc execCmd(command: string): int =
proc createFdSet(fd: var TFdSet, s: seq[Process], m: var int) =
proc pruneProcessSet(s: var seq[Process], fd: var TFdSet) =
proc select(readfds: var seq[Process], timeout = 500): int =
proc hasData*(p: Process): bool =
proc execCmdEx*(command: string, options: set[ProcessOption] = {
```

<p align="center">
    <img height="300" alt="OffensiveNim" src="https://user-images.githubusercontent.com/5151193/98487722-ed729600-21e1-11eb-9d77-a79b0f3634de.png">
</p>

# OffensiveNim

My experiments in weaponizing [Nim](https://nim-lang.org/) for implant development and general offensive operations.

## Table of Contents

- [OffensiveNim](#offensivenim)
  * [Why Nim?](#why-nim)
  * [Examples in this repo](#examples-in-this-repo)
  * [Compiling the examples](#compiling-the-examples-in-this-repo)
  * [Cross Compiling](#cross-compiling)
  * [Interfacing with C/C++](#interfacing-with-cc)
  * [Creating Windows DLLs with an exported DllMain](#creating-windows-dlls-with-an-exported-dllmain)
  * [Optimizing executables for size](#optimizing-executables-for-size)
  * [Executable size difference with the Winim Library](#executable-size-difference-when-using-the-winim-library-vs-without)
  * [Opsec Considirations](#opsec-considerations)
  * [Converting C Code to Nim](#converting-c-code-to-nim)
  * [Language Bridges](#language-bridges)
  * [Debugging](#debugging)
  * [Setting up a dev environment](#setting-up-a-dev-environment)
  * [Pitfalls I found myself falling into](#pitfalls-i-found-myself-falling-into)
  * [Interesting Nim Libraries](#interesting-nim-libraries)
  * [Nim for Implant Dev Links](#nim-for-implant-dev-links)

## Why Nim?

- Compiles *directly* to C, C++, Objective-C and Javascript.
- Since it doesn't rely on a VM/runtime does not produce what I like to call "T H I C C malwarez" as supposed to other languages (e.g. Golang)
- Python inspired syntax, allows rapid native payload creation & prototyping.
- Has **extremely** mature [FFI](https://nim-lang.org/docs/manual.html#foreign-function-interface) (Foreign Function Interface) capabilities.
- Avoids making you actually write in C/C++ and subsequently avoids introducing a lot of security issues into your software.
- Super easy cross compilation to Windows from Nix/MacOS, only requires you to install the `mingw` toolchain and passing a single flag to the nim compiler.
- The Nim compiler and the generated executables support all major platforms like Windows, Linux, BSD and macOS. Can even compile to Nintendo switch , IOS & Android. See the cross-compilation section in the [Nim compiler usage guide](https://nim-lang.github.io/Nim/nimc.html#crossminuscompilation)
- You could *technically* write your implant and c2 backend both in Nim as you can compile your code directly to Javascript. Even has some [initial support for WebAssembly's](https://forum.nim-lang.org/t/4779) 

## Examples in this Repo

| File | Description |
| ---  | --- |
| `pop_bin.nim` | Call `MessageBox` WinApi *without* using the Winim library |
| `pop_winim_bin.nim` | Call `MessageBox` *with* the Winim libary |
| `pop_winim_lib.nim` | Example of creating a Windows DLL with an exported `DllMain` |  
| `wmiquery_bin.nim` | Queries running processes and installed AVs using using WMI |
| `shellcode_bin.nim` | Creates a suspended process and injects shellcode with `VirtualAllocEx`/`CreateRemoteThread`. Also demonstrates the usage of compile time definitions to detect arch, os etc..| 
| `passfilter_lib.nim` | Log password changes to a file by (ab)using a password complexity filter |
| `minidump_bin.nim` | Creates a memory dump of lsass using `MiniDumpWriteDump` |
| `http_request_bin.nim` | Demonstrates a couple of ways of making HTTP requests |
| `execute_sct_bin.nim` | `.sct` file Execution via `GetObject()` |
| `scriptcontrol_bin.nim` | Dynamically execute VBScript and JScript using the `MSScriptControl` COM object | 
| `excel_com_bin.nim` | Injects shellcode using the Excel COM object and Macros |
| `clr_bin.nim` | Hosts the CLR and executes .NET assemblies (**WIP, help appreciated**) | 
| `amsi_bypass_bin.nim` | Patches AMSI out of the current process (**WIP, help appreciated**) |
| `excel_4_com_bin.nim` | Injects shellcode using the Excel COM object and Excel 4 Macros (**WIP**) |

## Compiling the examples in this repo

This repository does not provide binaries, you're gonna have to compile them yourself.

This repo was setup to cross-compile the example Nim source files to Windows from Nix/MacOS, however they should work just fine directly compiling them on Windows (Don't think you'll be able to use the Makefile tho which compiles them all in one go).

[Install Nim](https://nim-lang.org/install_unix.html) using your systems package manager (for windows [use the installer on the official website](https://nim-lang.org/install_windows.html))

- `brew install nim`
- `apt install nim`

(Nim also provides a docker image but don't know how it works when it comes to cross-compiling, need to look into this)

You should now have the `nim` & `nimble` commands available, the former is the Nim compiler and the latter is Nim's package manager.

Install the `Mingw` toolchain needed for cross-compilation to Windows (Not needed if you're compiling on Windows):
- Nix: `apt-get insatall mingw`
- MacOS: `brew install mingw-w64`

Finally, install the magnificent [Winim](https://github.com/khchen/winim) library:

- `nimble install winim`

Then cd into the root of this repository and run `make`.

You should find the binaries and dlls in the `bin/` directory

## Cross Compiling

See the cross-compilation section in the [Nim compiler usage guide](https://nim-lang.github.io/Nim/nimc.html#crossminuscompilation), for a lot more details.

Cross compiling to Windows from MacOs/Nix requires the `mingw` toolchain, usually a matter of just `brew install mingw` or `apt install mingw`.

You then just have to pass the  `-d=mingw` flag to the nim compiler.

E.g. `nim c -d=mingw --app=console --cpu=amd64 source.nim`

## Interfacing with C/C++

See the insane [FFI section](https://nim-lang.org/docs/manual.html#foreign-function-interface) in the Nim manual.

If you're familiar with csharps P/Invoke it's essentially the same concept albeit a looks a tad bit uglier:

Calling `MessageBox` example

```nim
type
    HANDLE* = int
    HWND* = HANDLE
    UINT* = int32
    LPCSTR* = cstring

proc MessageBox*(hWnd: HWND, lpText: LPCSTR, lpCaption: LPCSTR, uType: UINT): int32 
  {.discardable, stdcall, dynlib: "user32", importc: "MessageBoxA".}

MessageBox(0, "Hello, world !", "Nim is Powerful", 0)
```

For any complex Windows API calls use the [Winim library](https://github.com/khchen/winim), saves an insane amount of time and doesn't add too much to the executable size (see below) depending on how you import it.

Even has COM support!!!

## Creating Windows DLLs with an exported `DllMain`

Big thanks to the person who posted [this](https://forum.nim-lang.org/t/1973) on the Nim forum.

The Nim compiler tries to create a `DllMain` function for you automatically at compile time whenever you tell it to create a windows DLL, however, it doesn't actually export it for some reason. In order to have an exported `DllMain` you need to pass `--nomain` and define a `DllMain` function yourself with the appropriate pragmas (`stdcall, exportc, dynlib`).

You need to also call `NimMain` from your `DllMain` to initialize Nim's garbage collector. (Very important, otherwise your computer will literally explode).

Example:

```nim
import winim/lean

proc NimMain() {.cdecl, importc.}

proc DllMain(hinstDLL: HINSTANCE, fdwReason: DWORD, lpvReserved: LPVOID) : BOOL {.stdcall, exportc, dynlib.} =
  NimMain()
  
  if fdwReason == DLL_PROCESS_ATTACH:
    MessageBox(0, "Hello, world !", "Nim is Powerful", 0)

  return true
```

To compile:

```
nim c -d=mingw --app=lib --nomain --cpu=amd64 mynim.dll
```


## Optimizing executables for size

Taken from the [Nim's FAQ page](https://nim-lang.org/faq.html)

For the biggest size decrease use the following flags `-d:danger -d:strip --opt:size`

Additionally, I've found you can squeeze a few more bytes out by passing `--passc=-flto --passl=-flto` to the compiler. Also take a look at the `Makefile` in this repo.

These flags decrease sizes **dramatically**: the shellcode injection example goes from 484.3 KB to 46.5 KB when cross-compiled from MacOSX!

## Executable size difference when using the Winim library vs without

Incredibly enough the size difference is pretty negligible. Especially when you apply the size optimizations outlined above.

The two examples `pop_bin.nim` and `pop_winim_bin.nim` were created for this purpose.

The former defines the `MessageBox` WinAPI call manually and the latter uses the Winim library (specifically `winim/lean` which is only the core SDK, see [here](https://github.com/khchen/winim#usage)), results:

```
byt3bl33d3r@ecl1ps3 OffensiveNim % ls -lah bin
-rwxr-xr-x  1 byt3bl33d3r  25K Nov 20 18:32 pop_bin_32.exe
-rwxr-xr-x  1 byt3bl33d3r  32K Nov 20 18:32 pop_bin_64.exe
-rwxr-xr-x  1 byt3bl33d3r  26K Nov 20 18:33 pop_winim_bin_32.exe
-rwxr-xr-x  1 byt3bl33d3r  34K Nov 20 18:32 pop_winim_bin_64.exe
```

If you import the entire Winim library with `import winim/com` it adds only around ~20ish KB which considering the amount of functionality it abstracts is 100% worth that extra size:
```
byt3bl33d3r@ecl1ps3 OffensiveNim % ls -lah bin
-rwxr-xr-x  1 byt3bl33d3r  42K Nov 20 19:20 pop_winim_bin_32.exe
-rwxr-xr-x  1 byt3bl33d3r  53K Nov 20 19:20 pop_winim_bin_64.exe
```

## Opsec Considerations

Because of how Nim resolves DLLs dynamically using `LoadLibrary` using it's FFI none of your external imported functions will actually show up in the executables static imports (see [this blog post](https://secbytes.net/Implant-Roulette-Part-1:-Nimplant) for more on this):

![](https://user-images.githubusercontent.com/5151193/99911179-d0dd6000-2caf-11eb-933a-6a7ada510747.png)

If you compile Nim source to a DLL, seems like you'll always have an exported `NimMain`, no matter if you specify your own `DllMain` or not (??). This could potentially be used as a signature, don't know how many shops are actually using Nim in their development stack. Definitely stands out.

![](https://user-images.githubusercontent.com/5151193/99911079-4563cf00-2caf-11eb-960d-e500534b56dd.png)

## Converting C code to Nim

https://github.com/nim-lang/c2nim

Used it to translate a bunch of small C snippets, haven't tried anything major.

## Language Bridges

  - Python integration https://github.com/yglukhov/nimpy
    * This is 
