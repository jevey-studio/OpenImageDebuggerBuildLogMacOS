# OpenImageDebuggerBuildLogMacOS
A log for what ive done on building this on a M1 chip machine

## What's OpenImageDebugger?
Link [here](https://github.com/OpenImageDebugger/OpenImageDebugger.git) 

## What's my issue?
Tried to build it in MacOS but failed many items, so better mark them down.

## Steps to fix it
First, DO NOT use qmake instruction from [here]()
But add **pkg_config** correctly pls
And back to main page, use the CMake way! 


## Install Qt, Qt5 from Homebrew
Just install it and make sure ~/.zshrc added with paths

## What's in my ~/.zshrc
```
export PATH="/opt/homebrew/opt/qt@5/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/opt/qt@5/lib"
export CPPFLAGS="-I/opt/homebrew/opt/qt@5/include"
export PKG_CONFIG_PATH=/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/pkgconfig
```

## What's my cmake commands
```
cmake -S . -B build -DCMAKE_INSTALL_PREFIX=/path/to/your/install/dir \
-DPYTHON_INCLUDE_DIR=/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/include/python3.9 \
-DPYTHON_LIBRARIES=/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib \
-DPYTHON_EXECUTABLE=/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
```
Make sure that -DPYTHON_EXECUTABLE not -DPYTHON3_EXECUTABLE is called in due to what i was inspired [here](https://www.reddit.com/r/cmake/comments/otxfb4/cmake_finds_the_python_interpreter_yet_says_it/).
The reason why i didn't use this [fix](https://stackoverflow.com/questions/24174394/cmake-is-not-able-to-find-python-libraries) is that the output didn't contain any files for my local python.  

## What's my cmake output
```
-- The C compiler identification is AppleClang 15.0.0.15000040
-- The CXX compiler identification is AppleClang 15.0.0.15000040
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Found OpenGL: /Library/Developer/CommandLineTools/SDKs/MacOSX14.0.sdk/System/Library/Frameworks/OpenGL.framework (Required is at least version "2.1")  
CMake Warning (dev) at src/oidbridge/CMakeLists.txt:28 (find_package):
  Policy CMP0148 is not set: The FindPythonInterp and FindPythonLibs modules
  are removed.  Run "cmake --help-policy CMP0148" for policy details.  Use
  the cmake_policy command to set the policy and suppress this warning.

This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found PythonLibs: /Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/libpython3.9.dylib (found suitable version "3.9.6", minimum required is "3.8.10") 
-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/path/to/your/OpenImageDebugger/build
```

## What's my build output?
Run **cmake --build build --config Release --target install -j 4** and then:
```
[  0%] Built target oidbridge_python3_autogen_timestamp_deps
[  0%] Built target oidwindow_autogen_timestamp_deps
[  4%] Automatic MOC and UIC for target oidwindow
[  4%] Automatic MOC and UIC for target oidbridge_python3
[  4%] Built target oidbridge_python3_autogen
[  6%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/__/debuggerinterface/python_native_interface.cpp.o
[  8%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/oidbridge_python3_autogen/mocs_compilation.cpp.o
[ 10%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/oid_bridge.cpp.o
[ 12%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/__/ipc/message_exchange.cpp.o
[ 14%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/__/ipc/raw_data_decode.cpp.o
[ 14%] Built target oidwindow_autogen
[ 17%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/__/system/process/process.cpp.o
[ 19%] Automatic RCC for resources/resources.qrc
[ 21%] Building CXX object src/oidbridge/CMakeFiles/oidbridge_python3.dir/__/system/process/process_unix.cpp.o
[ 23%] Building CXX object src/CMakeFiles/oidwindow.dir/oidwindow_autogen/mocs_compilation.cpp.o
[ 25%] Building CXX object src/CMakeFiles/oidwindow.dir/oid_window.cpp.o
[ 27%] Building CXX object src/CMakeFiles/oidwindow.dir/io/buffer_exporter.cpp.o
[ 29%] Linking CXX shared library liboidbridge_python3.dylib
[ 31%] Built target oidbridge_python3
[ 34%] Building CXX object src/CMakeFiles/oidwindow.dir/ipc/message_exchange.cpp.o
[ 36%] Building CXX object src/CMakeFiles/oidwindow.dir/ipc/raw_data_decode.cpp.o
[ 38%] Building CXX object src/CMakeFiles/oidwindow.dir/math/linear_algebra.cpp.o
[ 40%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/decorated_line_edit.cpp.o
[ 42%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/gl_canvas.cpp.o
[ 44%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/gl_text_renderer.cpp.o
[ 46%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/go_to_widget.cpp.o
/Users/path/to/your/install/dir/OpenImageDebugger/src/ui/gl_text_renderer.cpp:83:5: warning: 'glBindTexture' is deprecated: first deprecated in macOS 10.14 - OpenGL API deprecated. (Define GL_SILENCE_DEPRECATION to silence these warnings) [-Wdeprecated-declarations]
    glBindTexture(GL_TEXTURE_2D, text_tex);
    ^
/Library/Developer/CommandLineTools/SDKs/MacOSX14.0.sdk/System/Library/Frameworks/OpenGL.framework/Headers/gl.h:2386:13: note: 'glBindTexture' has been explicitly marked deprecated here
extern void glBindTexture (GLenum target, GLuint texture) OPENGL_DEPRECATED(10.0, 10.14);
            ^
1 warning generated.
[ 48%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/main_window/auto_contrast.cpp.o
[ 51%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/main_window/initialization.cpp.o
[ 53%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/main_window/main_window.cpp.o
[ 55%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/main_window/message_processing.cpp.o
[ 57%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/main_window/ui_events.cpp.o
[ 59%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/symbol_completer.cpp.o
[ 61%] Building CXX object src/CMakeFiles/oidwindow.dir/ui/symbol_search_input.cpp.o
[ 63%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/components/background.cpp.o
[ 65%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/components/buffer.cpp.o
[ 68%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/components/buffer_values.cpp.o
[ 70%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/components/camera.cpp.o
[ 72%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/components/component.cpp.o
[ 74%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/events.cpp.o
/Users/path/to/your/install/dir/OpenImageDebugger/src/visualization/components/buffer_values.cpp:224:5: warning: 'glBindTexture' is deprecated: first deprecated in macOS 10.14 - OpenGL API deprecated. (Define GL_SILENCE_DEPRECATION to silence these warnings) [-Wdeprecated-declarations]
    glBindTexture(GL_TEXTURE_2D, buff_tex);
    ^
/Library/Developer/CommandLineTools/SDKs/MacOSX14.0.sdk/System/Library/Frameworks/OpenGL.framework/Headers/gl.h:2386:13: note: 'glBindTexture' has been explicitly marked deprecated here
extern void glBindTexture (GLenum target, GLuint texture) OPENGL_DEPRECATED(10.0, 10.14);
            ^
/Users/path/to/your/install/dir/OpenImageDebugger/src/visualization/components/buffer_values.cpp:228:5: warning: 'glBindTexture' is deprecated: first deprecated in macOS 10.14 - OpenGL API deprecated. (Define GL_SILENCE_DEPRECATION to silence these warnings) [-Wdeprecated-declarations]
    glBindTexture(GL_TEXTURE_2D, text_renderer->text_tex);
    ^
/Library/Developer/CommandLineTools/SDKs/MacOSX14.0.sdk/System/Library/Frameworks/OpenGL.framework/Headers/gl.h:2386:13: note: 'glBindTexture' has been explicitly marked deprecated here
extern void glBindTexture (GLenum target, GLuint texture) OPENGL_DEPRECATED(10.0, 10.14);
            ^
[ 76%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/game_object.cpp.o
2 warnings generated.
[ 78%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shader.cpp.o
[ 80%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/background_fs.cpp.o
[ 82%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/background_vs.cpp.o
[ 85%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/buffer_fs.cpp.o
[ 87%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/buffer_vs.cpp.o
[ 89%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/text_fs.cpp.o
[ 91%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/shaders/text_vs.cpp.o
[ 93%] Building CXX object src/CMakeFiles/oidwindow.dir/visualization/stage.cpp.o
[ 95%] Building CXX object src/CMakeFiles/oidwindow.dir/oidwindow_autogen/3YJK5W5UP7/qrc_resources.cpp.o
[ 97%] Linking CXX executable oidwindow
[100%] Built target oidwindow
Install the project...
-- Install configuration: ""
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidwindow
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/liboidbridge_python3.dylib
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/matlab
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/matlab/oid_load.m
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/events.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/__init__.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/test.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/symbols.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/ides
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/ides/__init__.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/ides/qtcreator.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/debuggers
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/debuggers/interfaces.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/debuggers/__init__.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/debuggers/lldbbridge.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/debuggers/gdbbridge.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/sysinfo.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidwindow.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/typebridge.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidtypes
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidtypes/interface.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidtypes/__init__.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidtypes/opencv.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oidscripts/oidtypes/eigen3.py
-- Installing: /Users/path/to/your/install/dir/InstallOID/OpenImageDebugger/oid.py

```

Note **/Users/path/to/your/install/dir/** should be changed to your local path on your Mac. 

**Good luck and enjoy!**
