---
date: 2021-03-17T14:32:00-04:00
description: ""
featured_image: ""
tags: []
title: "Visual Studio Code C/C++ Extension Setup on M1 Mac"
featured_image: '/images/log_06/m1-chip.jpg'
---

## Prerequisites
- Visual Studio Code extension `ms-vscode.cpptools`

- Xcode Command-line Tools / Xcode IDE 
	- To install Xcode Command-line Tools: `xcode-select --install`
	- To install Xcode IDE: `mas install Xcode`

### Test Environment
- macOS 11.2.3

	```Bash
	$ uname -v
	Darwin Kernel Version 20.3.0: Thu Jan 21 00:06:51 PST 2021; 
	root:xnu-7195.81.3~1/RELEASE_ARM64_T8101
	```
	
- Xcode 12.4 (12D4e)
	
	```Bash
	$ clang --version
	Apple clang version 12.0.0 (clang-1200.0.32.29)
	Target: arm64-apple-darwin20.3.0
	Thread model: posix
	InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
	```
	
- Visual Studio Code 1.54.3 (Universal)
	- Commit: 2b9aebd5354a3629c3aba0a5f5df49f43d6689f8
	- Extension: ms-vscode.cpptools v1.2.2  

## Setup

Modify the `settings.json` in Visual Studio Code as shown below

### Compiler path
- For Xcode Command-line Tools: 

	```
	/Library/Developer/CommandLineTools/usr/bin
	```
- For Xcode IDE:
	
	```
	/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/
	```
	
### Mac framwork path
> The system header files

- For Xcode Command-line Tools:

	```
	/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks
	``` 
- For Xcode IDE:
	
	```
	/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks
	```

### Include path
- For Xcode Command-line Tools:
	
	```
	/usr/local/include/**
	/Library/Developer/CommandLineTools/usr/lib/clang/12.0.0/include/**
	/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/**
	/Library/Developer/CommandLineTools/usr/include/**
	```
	
- For Xcode IDE:

	```
	/usr/local/include/**
	/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/12.0.0/include/**
	/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/**
	/Applications/Xcode.app/Contents/Developer/usr/lib/llvm-gcc/4.2.1/include/**
	```

## Example

The version of `clang` may vary, using `clang --version` to check the current version 

- If only Xcode Command-line Tools is installed

	```JSON
	"C_Cpp.default.systemIncludePath": [
	  "/usr/local/include/**",
	  "/Library/Developer/CommandLineTools/usr/lib/clang/12.0.0/include/**",
	  "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/**",
	  "/Library/Developer/CommandLineTools/usr/include/**",
	  "${workspaceFolder}/**"
	],
	"C_Cpp.default.macFrameworkPath": [
	  "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks"
	],
	"C_Cpp.default.compilerPath": "/usr/bin/clang",
	"C_Cpp.default.intelliSenseMode": "macos-clang-arm64",
	"C_Cpp.default.cStandard": "c11",     // or any version preferred
	"C_Cpp.default.cppStandard": "c++11"  // or any version preferred
	```

- If Xcode IDE is installed

	```JSON
	"C_Cpp.default.systemIncludePath": [
	  "/usr/local/include/**",
	  "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/12.0.0/include/**",
	  "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/**",
	  "/Applications/Xcode.app/Contents/Developer/usr/lib/llvm-gcc/4.2.1/include/**",
	  "${workspaceFolder}/**"
	],
	"C_Cpp.default.macFrameworkPath": [
	  "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks"
	],
	"C_Cpp.default.compilerPath": "/usr/bin/clang",
	"C_Cpp.default.intelliSenseMode": "macos-clang-arm64",
	"C_Cpp.default.cStandard": "c11",     // or any version preferred
	"C_Cpp.default.cppStandard": "c++11"  // or any version preferred
	```

## Other
All `include` directories found under `Xcode.app/Contents/Developer/`

```Bash
$ find . -name "include"
./usr/lib/llvm-gcc/4.2.1/include
./usr/share/xcs/CouchDB/lib/couchdb/erlang/lib/couch-1.7.1/include
./usr/share/xcs/CouchDB/lib/couchdb/erlang/lib/couch_mrview-0.1/include
./usr/share/xcs/CouchDB/lib/couchdb/erlang/lib/couch_replicator-0.1/include
./Platforms/AppleTVOS.platform/Developer/SDKs/AppleTVOS.sdk/usr/include
./Platforms/AppleTVOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/tvOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/PrivateFrameworks/GPUCompiler.framework/Libraries/lib/clang/31001.50/include
./Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include
./Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/PrivateFrameworks/GPUCompiler.framework/Libraries/lib/clang/31001.50/include
./Platforms/WatchOS.platform/Developer/SDKs/WatchOS.sdk/usr/include
./Platforms/WatchOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/watchOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/PrivateFrameworks/GPUCompiler.framework/Libraries/lib/clang/31001.50/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/PrivateFrameworks/GPUCompiler.framework/Versions/A/lib/clang/3.5/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/OpenCL.framework/Versions/A/lib/clang/3.2/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/numpy/core/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/numpy/numarray/include
./Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/PyObjC/pyobjc_core-2.5.1-py2.7.egg-info/include
./Platforms/MacOSX.platform/Developer/SDKs/DriverKit20.2.sdk/System/DriverKit/usr/include
./Platforms/WatchSimulator.platform/Developer/SDKs/WatchSimulator.sdk/usr/include
./Platforms/AppleTVSimulator.platform/Developer/SDKs/AppleTVSimulator.sdk/usr/include
./Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/usr/include
./Library/Frameworks/Python3.framework/Versions/3.8/include
./Toolchains/XcodeDefault.xctoolchain/usr/metal/macos/lib/clang/31001.160/include
./Toolchains/XcodeDefault.xctoolchain/usr/metal/ios/lib/clang/31001.50/include
./Toolchains/XcodeDefault.xctoolchain/usr/include
./Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/12.0.0/include
./Toolchains/XcodeDefault.xctoolchain/usr/lib/tapi/12.0.0/include
```

## Reference

- Blog: [Xcode install (on MacOS)](https://wilsonmar.github.io/xcode/)
- git memory: [clang-arm intelliSenseMode selected instead of clang-arm64 on Apple Silicon machine](https://www.gitmemory.com/issue/microsoft/vscode-cmake-tools/1583/739054834)
- Microsoft Visual Stuio Code Tutorial: [Using Clang in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-clang-mac)
- Github Gist: [yamaya / xcode-clang-vers](https://gist.github.com/yamaya/2924292)