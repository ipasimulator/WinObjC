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
    EbrDevRandomFile.cpp
    RawBuffer.cpp
    EbrStorageFile.cpp
    PathMapper.cpp
    AssetFile.cpp)

add_library (Starboard SHARED ${SOURCE_FILES})

add_dependencies (Starboard prep winobjc-Logging)

target_include_directories (Starboard PUBLIC
    ../../include
    ../include
    ../include/xplat
    # TODO: Where do these come from?
    ../../include/xplat
    ../../tools/include/xplat
    # Logging include directories. See `tools/Logging/CMakeLists.txt`.
    ../../tools/Logging/include
    ../../tools/Logging/include/wil
    ../../tools/include/WOCStdLib)
