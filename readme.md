# CrossWindow

[![cmake-img]][cmake-url]
[![License][license-img]][license-url]
[![Unit Tests][travis-img]][travis-url]
[![Coverage Tests][codecov-img]][codecov-url]

A basic cross platform system abstraction library for managing windows and performing OS tasks. 

## Features

- ⚪ Window Creation

- ⌨️ 🖱️ 👆 Basic Input (Keyboard, Mouse, Touch)

- 👻 Platform specific features (Mac Transparency, Touch Bar, etc.)

## Supports

<table style="width:100%">
<tbody>
<tr>
<td align="center"><a target="_blank" href="docs/images/windows.svg"><img src="docs/images/windows.svg" alt="Windows" style="max-width:100%;"></a></td>
<td align="center"><a target="_blank" href="docs/images/macos.svg"><img src="docs/images/macos.svg" alt="MacOS" style="max-width:100%;"></a></td>
<td align="center"><a target="_blank" href="docs/images/linux.svg"><img src="docs/images/linux.svg" alt="Linux" style="max-width:100%;"></a></td>
</tr>
<tr>
<td align="center"><g-emoji class="g-emoji" alias="framed_picture" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f5bc.png">🖼️</g-emoji> Windows</td>
<td align="center"><g-emoji class="g-emoji" alias="apple" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f34e.png">🍎</g-emoji> Mac</td>
<td align="center"><g-emoji class="g-emoji" alias="penguin" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f427.png">🐧</g-emoji> Linux (In Progress)</td>
</tr>
<tr>
<td align="center"><a target="_blank" href="docs/images/android.svg"><img src="docs/images/android.svg" alt="Android" style="max-width:100%;"></a></td>
<td align="center"><a target="_blank" href="docs/images/ios.svg"><img src="docs/images/ios.svg" alt="iOS" style="max-width:100%;"></a></td>
<td align="center"><a target="_blank" href="docs/images/webassembly.svg"><img src="docs/images/webassembly.svg" alt="WebAssembly" style="max-width:100%;"></a></td>
</tr>
<tr>
<td align="center"><g-emoji class="g-emoji" alias="robot" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f916.png">🤖</g-emoji> Android (In Progress)</td>
<td align="center"><g-emoji class="g-emoji" alias="iphone" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f4f1.png">📱</g-emoji> iOS (In Progress)</td>
<td align="center"><g-emoji class="g-emoji" alias="globe_with_meridians" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f310.png">🌐</g-emoji> WebAssembly (In Progress)</td>
</tr></tbody></table>




## Installation

First add the repo as a submodule in your dependencies folder such as `external/`:

```
cd external
git submodule add https://github.com/alaingalvan/CrossWindow.git
```

Then in your `CMakeLists.txt` file, include the following:

```cmake
# ⬇ Add your dependency:
add_subdirectories(external/CrossWindow)

# ❎ When creating your executable use CrossWindow's abstraction function:
xwin_add_executable(
    # Target
    ${PROJECT_NAME}
    # Source Files (make sure to surround in quotations so CMake treats it as a list)
    "${SOURCE_FILES}"
)

# 🔗 Link CrossWindow to your project:
target_link_libraries(
    ${PROJECT_NAME}
    CrossWindow
)
```

## Usage

Then create a main delegate function `void xmain(int argc, const char** argv)` in a `.cpp` file in your project (for example "`XMain.cpp`") where you'll put your application logic:

```cpp
#include "CrossWindow/CrossWindow.h"

void xmain(int argc, const char** argv)
{
    // 🖼️ Create Window Description
    xwin::WindowDesc windowDesc;
    windowDesc.name = "Test";
    windowDesc.title = "My Title";
    windowDesc.visible = true;
    windowDesc.width = 1280;
    windowDesc.height = 720;

    bool closed = false;
    
    // ⚪ Initialize
    xwin::Window window;
    xwin::EventQueue eventQueue;

    if (!window.create(windowDesc, eventQueue))
    { return; }

    // 🏁 Engine loop
    bool isRunning = true;

    while (isRunning)
    {
        // ♻️ Update the event queue
        eventQueue.update();

        // 🎈 Iterate through that queue:
        while (!eventQueue.empty())
        {
            const xwin::Event event = eventQueue.pop();
            switch(event.type)
            {
            case xwin::EventType::MouseMove:
            xwin::MouseData* mouse = static_cast<xwin::MouseData*>(e.data);
            //mouse.x, mouse.y
            break;
            case xwin::EventType::Close:
            window.close();
            break;
            default:
            // Do nothing
            }
        }
    }
}
```

This `xmain` function will be called from a *platform specific main function* that will be included in your main project by CMake. If you ever need to access something from the platform specific main function for whatever reason, you'll find it in `xwin::getXWinState()`.

For more examples visit the [Documentation](/docs).

## Development

Be sure to have [CMake](https://cmake.org) Installed.

| CMake Options | Description |
|:-------------:|:-----------:|
| `XWIN_TESTS` | Whether or not unit tests are enabled. Defaults to `OFF`, Can be `ON` or `OFF`. |
| `OPERATING_SYSTEM` | What Operating System to build for, defaults to `AUTO`, can be `NOOP`, `WINDOWS`, `MACOS`, `LINUX`, `ANDROID`, `IOS`, `WASM`. |

We would recommend making a folder where solution files will be built to to avoid making your file system look too messy, such as `visualstudio/` or `xcode/` depending on the platform you're building for. `cd` to that directory and type in your terminal:

```bash
# 🖼️ To build your Visual Studio solution on Windows
cd crosswindow
mkdir visualstudio
cmake -DXWIN_TESTS=ON .. -A x64

# 🍎 To build your XCode project On Mac OS
cd crosswindow
mkdir xcode
cmake -G Xcode -DXWIN_TESTS=ON ..
```

Whenever you add new files to the project, run `cmake ..` from your solution/project folder, and if you edit the `CMakeLists.txt` file be sure to delete your `CMakeCache.txt` and `CMakeFiles/` and run Cmake again.

## License

CrossWindow is licensed as either **MIT** or **Apache-2.0**, whichever you would prefer.

[cmake-img]: https://img.shields.io/badge/cmake-3.9-1f9948.svg?style=flat-square
[cmake-url]: https://cmake.org/
[license-img]: https://img.shields.io/:license-mit-blue.svg?style=flat-square
[license-url]: https://opensource.org/licenses/MIT
[travis-img]: https://img.shields.io/travis/alaingalvan/crosswindow.svg?style=flat-square
[travis-url]: https://travis-ci.org/alaingalvan/crosswindow
[codecov-img]:https://img.shields.io/codecov/c/github/alaingalvan/crosswindow.svg?style=flat-square
[codecov-url]: https://codecov.io/gh/alaingalvan/crosswindow
