[![license](https://img.shields.io/badge/license-BSD_3-brightgreen.svg?style=flat)](https://github.com/Tencent/MMKV/blob/master/LICENSE.TXT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/Tencent/MMKV/pulls)
[![Release Version](https://img.shields.io/badge/release-1.0.18-brightgreen.svg)](https://github.com/Tencent/MMKV/releases)
[![Platform](https://img.shields.io/badge/Platform-%20iOS%20%7C%20Android-brightgreen.svg)](https://github.com/Tencent/MMKV/wiki/home)

中文版本请参看[这里](./readme_cn.md)

MMKV is an **efficient**, **small**, **easy-to-use** mobile key-value storage framework used in the WeChat application. It's currently available on **iOS**, **macOS**, **Android** and **Windows**.

# MMKV for iOS/macOS

## Features

* **Efficient**. MMKV uses mmap to keep memory synced with file, and protobuf to encode/decode values, making the most of iOS/macOS to achieve best performance.
 
* **Easy-to-use**. You can use MMKV as you go, no configurations needed. All changes are saved immediately, no `synchronize` calls needed.

* **Small**.
  * **A handful of files**: MMKV contains encode/decode helpers and mmap logics and nothing more. It's really tidy.
  * **Less than 30K in binary size**: MMKV adds less than 30K per architecture on App size, and much less when zipped (ipa).

## Getting Started

### Installation Via CocoaPods:
  1. Install [CocoaPods](https://guides.CocoaPods.org/using/getting-started.html);
  2. Open terminal, `cd` to your project directory, run `pod repo update` to make CocoaPods aware of the latest available MMKV versions;
  3. Edit your Podfile, add `pod 'MMKV'` to your app target;
  4. Run `pod install`;
  5. Open the `.xcworkspace` file generated by CocoaPods;
  6. Add `#import <MMKV/MMKV.h>` to your source file and we are done.

For other installation options, see [iOS/macOS Setup](https://github.com/Tencent/MMKV/wiki/iOS_setup).

### Quick Tutorial
You can use MMKV as you go, no configurations needed. All changes are saved immediately, no `synchronize` calls needed.

```objective-c
MMKV *mmkv = [MMKV defaultMMKV];
    
[mmkv setBool:YES forKey:@"bool"];
BOOL bValue = [mmkv getBoolForKey:@"bool"];
    
[mmkv setInt32:-1024 forKey:@"int32"];
int32_t iValue = [mmkv getInt32ForKey:@"int32"];
    
[mmkv setString:@"hello, mmkv" forKey:@"string"];
NSString *str = [mmkv getStringForKey:@"string"];
```

Full tutorials can be found [here](https://github.com/Tencent/MMKV/wiki/iOS_tutorial).

## Performance
Writing random `int` for 10000 times, we get this chart:  
![](https://github.com/Tencent/MMKV/wiki/assets/profile_mini.jpg)  
For more benchmark data, please refer to [our benchmark](https://github.com/Tencent/MMKV/wiki/iOS_benchmark).

# MMKV for Android

## Features

* **Efficient**. MMKV uses mmap to keep memory synced with file, and protobuf to encode/decode values, making the most of Android to achieve best performance.
  * **Multi-Process concurrency**: MMKV supports concurrent read-read and read-write access between processes.

* **Easy-to-use**. You can use MMKV as you go. All changes are saved immediately, no `sync`, no `apply` calls needed.

* **Small**.
  * **A handful of files**: MMKV contains process locks, encode/decode helpers and mmap logics and nothing more. It's really tidy.
  * **About 60K in binary size**: MMKV adds about 60K per architecture on App size, and much less when zipped (apk).


## Getting Started

### Installation Via Maven
Add the following lines to `build.gradle` on your app module:

```gradle
dependencies {
    implementation 'com.tencent:mmkv:1.0.18'
    // replace "1.0.18" with any available version
}
```

For other installation options, see [Android Setup](https://github.com/Tencent/MMKV/wiki/android_setup).

### Quick Tutorial
You can use MMKV as you go. All changes are saved immediately, no `sync`, no `apply` calls needed.  
Setup MMKV on App startup, say your `MainActivity`, add these lines:

```Java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    String rootDir = MMKV.initialize(this);
    System.out.println("mmkv root: " + rootDir);
    //……
}
```

MMKV has a global instance, that can be used directly:

```Java
import com.tencent.mmkv.MMKV;
    
MMKV kv = MMKV.defaultMMKV();

kv.encode("bool", true);
boolean bValue = kv.decodeBool("bool");

kv.encode("int", Integer.MIN_VALUE);
int iValue = kv.decodeInt("int");

kv.encode("string", "Hello from mmkv");
String str = kv.decodeString("string");
```

MMKV also supports **Multi-Process Access**. Full tutorials can be found here [Android Tutorial](https://github.com/Tencent/MMKV/wiki/android_tutorial).

## Performance
Writing random `int` for 1000 times, we get this chart:  
![](https://github.com/Tencent/MMKV/wiki/assets/profile_android_mini.jpg)  
For more benchmark data, please refer to [our benchmark](https://github.com/Tencent/MMKV/wiki/android_benchmark).

# MMKV for Windows

## Features

* **Efficient**. MMKV uses mmap to keep memory synced with file, and protobuf to encode/decode values, making the most of Windows to achieve best performance.
  * **Multi-Process concurrency**: MMKV supports concurrent read-read and read-write access between processes.

* **Easy-to-use**. You can use MMKV as you go. All changes are saved immediately, no `save`, no `sync` calls needed.

* **Small**.
  * **A handful of files**: MMKV contains process locks, encode/decode helpers and mmap logics and nothing more. It's really tidy.
  * **About 10K in binary size**: MMKV adds about 10K on application size, and much less when zipped.


## Getting Started

### Installation Via Source
1. Getting source code from git repository:
  
   ```
   git clone https://github.com/Tencent/MMKV.git
   ```
  
2. Add `Win32/MMKV/MMKV.vcxproj` to your solution;
3. Add `MMKV` project to your project's dependencies;
4. Add `$(OutDir)include` to your project's `C/C++` -> `General` -> `Additional Include Directories`;
5. Add `$(OutDir)` to your project's `Linker` -> `General` -> `Additional Library Directories`;
6. Add `MMKV.lib` to your project's `Linker` -> `Input` -> `Additional Dependencies`;
7. Add `#include <MMKV/MMKV.h>` to your source file and we are done.


note:  

1. MMKV is compiled with `MT/MTd` runtime by default. If your project uses `MD/MDd`, you should change MMKV's setting to match your project's (`C/C++` -> `Code Generation` -> `Runtime Library`), or wise versa.
2. MMKV is developed with Visual Studio 2017, change the `Platform Toolset` if you use a different version of Visual Studio.

For other installation options, see [Windows Setup](https://github.com/Tencent/MMKV/wiki/windows_setup).

### Quick Tutorial
You can use MMKV as you go. All changes are saved immediately, no `sync`, no `save` calls needed.  
Setup MMKV on App startup, say in your `main()`, add these lines:

```C++
#include <MMKV/MMKV.h>

int main() {
    std::wstring rootDir = getYourAppDocumentDir();
    MMKV::initializeMMKV(rootDir);
    //...
}
```

MMKV has a global instance, that can be used directly:

```C++
auto mmkv = MMKV::defaultMMKV();

mmkv->set(true, "bool");
std::cout << "bool = " << mmkv->getBool("bool") << std::endl;

mmkv->set(1024, "int32");
std::cout << "int32 = " << mmkv->getInt32("int32") << std::endl;

mmkv->set("Hello, MMKV for Win32", "string");
std::string result;
mmkv->getString("string", result);
std::cout << "string = " << result << std::endl;
```

MMKV also supports **Multi-Process Access**. Full tutorials can be found here [Windows Tutorial](https://github.com/Tencent/MMKV/wiki/windows_tutorial).

## License
MMKV is published under the BSD 3-Clause license. For details check out the [LICENSE.TXT](./LICENSE.TXT).

## Change Log
Check out the [CHANGELOG.md](./CHANGELOG.md) for details of change history.

## Contributing

If you are interested in contributing, check out the [CONTRIBUTING.md](./CONTRIBUTING.md), also join our [Tencent OpenSource Plan](https://opensource.tencent.com/contribution).

To give clarity of what is expected of our members, MMKV has adopted the code of conduct defined by the Contributor Covenant, which is widely used. And we think it articulates our values well. For more, check out the [Code of Conduct](./CODE_OF_CONDUCT.md).

## FAQ & Feedback
Check out the [FAQ](https://github.com/Tencent/MMKV/wiki/FAQ) first. Should there be any questions, don't hesitate to create [issues](https://github.com/Tencent/MMKV/issues).
