if (CL_COMPILER)
    set (SOURCE_FILES
        RawBuffer.cpp
        EbrStorageFile.cpp
        PathMapper.cpp
        AssetFile.cpp)
else (CL_COMPILER)
    set (SOURCE_FILES
        #pthread.cpp # We use win-pthreads instead.
        CommonDigest.cpp
        CommonHMAC.cpp
        MurmurHash3.cpp
        pevents.cpp
        String.cpp
        PlatformSupport.cpp
        CommonCryptor.cpp
        DNSService.cpp
        EbrFile.cpp
        EbrIOFile.cpp
        EbrDevRandomFile.cpp)
endif (CL_COMPILER)

if (CL_COMPILER)
    add_library (Starboard STATIC ${SOURCE_FILES})
else (CL_COMPILER)
    add_library (Starboard SHARED ${SOURCE_FILES})
    add_prep_dep (Starboard)
endif (CL_COMPILER)

target_link_libraries (Starboard PUBLIC
    WindowsApp.lib
    msvcrtd.lib
    pthread)
if (CL_COMPILER)
    target_link_libraries (Starboard PUBLIC
        WOCStdLib
        Logging)
else (CL_COMPILER)
    target_link_libraries (Starboard PUBLIC
        woc-WOCStdLib
        woc-Logging
        woc-Starboard
        PRIVATE
        -Wl,/force:multiple,/debug,/nodefaultlib:libcmt
        "-Wl,/def:${CMAKE_CURRENT_SOURCE_DIR}/../../build/Starboard/dll/\
Starboard.def")
endif (CL_COMPILER)

target_include_directories (Starboard PUBLIC
    ../../include
    ../include
    ../include/xplat
    # TODO: Where do these come from?
    ../../include/xplat
    ../../tools/include/xplat
    ../../tools/deps/prebuilt/include) # Currently only for `dispatch`

target_compile_definitions (Starboard PUBLIC
    # These come from `clang.command.1.tlog` (i.e., file generated when building
    # WinObjC using MSBuild).
    WINAPI_FAMILY=WINAPI_FAMILY_APP
    _WINSOCK_DEPRECATED_NO_WARNINGS
    STARBOARD_PORT=1
    __WRL_NO_DEFAULT_LIB__
    WINOBJC
    _HAS_EXCEPTIONS=0
    OBJC_PORT
    SB_IMPEXP=
    # TODO: Include these conditionally (on CMAKE_BUILD_TYPE) and everywhere.
    _DEBUG
    _MT
    _DLL
    # TODO: Use this from `CommonVariables.cmake`.
    DISABLE_WINRT_DEPRECATION)

if (NOT CL_COMPILER)
    target_compile_options (Starboard PUBLIC
        -fobjc-runtime=ios-11
        # Warnings
        -Wno-c++17-extensions -Wno-nullability-completeness
        -Wno-unused-command-line-argument -Wno-c++17-compat-mangling
        -Wno-microsoft)
endif (NOT CL_COMPILER)
