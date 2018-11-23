if (CL_COMPILER)
    set (SOURCE_FILES
        RawBuffer.cpp
        EbrStorageFile.cpp
        PathMapper.cpp
        AssetFile.cpp)
else (CL_COMPILER)
    set (SOURCE_FILES
        #pthread.cpp # TODO: Use win-pthreads.
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
endif (CL_COMPILER)

if (NOT CL_COMPILER)
    add_dependencies (Starboard prep winobjc-Logging winobjc-Starboard)
    target_link_libraries (Starboard
        WindowsApp.lib ws2_32.lib
        "${WINOBJC_CMAKE_DIR}/tools/Logging/Logging.lib"
        "${WINOBJC_CMAKE_DIR}/Frameworks/Starboard/Starboard.lib")
    # TODO: Linking still fails, we must add more libraries...
endif (NOT CL_COMPILER)

target_include_directories (Starboard PUBLIC
    ../../include
    ../include
    ../include/xplat
    # TODO: Where do these come from?
    ../../include/xplat
    ../../tools/include/xplat
    ../../tools/deps/prebuilt/include # Currently only for `dispatch`.
    # Logging include directories. See `tools/Logging/CMakeLists.txt`.
    ../../tools/Logging/include
    ../../tools/Logging/include/wil
    ../../tools/include/WOCStdLib)

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
    # Without this, there is an error in header `Windows.UI.Notifications.h`.
    # TODO: Don't define this, rather use older SDK (e.g., the one we used when
    # we successfully built WinObjC using MSBuild, i.e., 10.0.14393.0).
    DISABLE_WINRT_DEPRECATION)

if (NOT CL_COMPILER)
    target_compile_options (Starboard PUBLIC
        -fobjc-runtime=ios-11
        # Warnings
        -Wno-c++17-extensions -Wno-nullability-completeness
        -Wno-unused-command-line-argument -Wno-c++17-compat-mangling
        -Wno-microsoft)
endif (NOT CL_COMPILER)
