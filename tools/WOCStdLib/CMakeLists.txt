set (SOURCE_FILES
    UTSName.cpp
    mach.cpp
    String.cpp
    TaskInfo.cpp
    unistd.cpp
    sched.cpp
    sysctl.cpp
    version.cpp
    dlfcn.cpp
    OSMisc.cpp
    OSEndian.cpp
    inet.cpp
    random.cpp
    time.cpp
    OSSpinLock.cpp)
list (TRANSFORM SOURCE_FILES PREPEND lib/)

add_library (WOCStdLib SHARED ${SOURCE_FILES})

add_dependencies (WOCStdLib prep)

target_include_directories (WOCStdLib PUBLIC
    ../include/WOCStdLib
    ../include)

target_link_libraries (WOCStdLib Logging WindowsApp.lib
    "-def:${SOURCE_DIR}/deps/WinObjC/tools/WOCStdLib/dll/WOCStdLib.def")

target_compile_definitions (WOCStdLib PUBLIC WINAPI_FAMILY=WINAPI_FAMILY_APP)
