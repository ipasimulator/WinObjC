set (SOURCE_FILES
    lib/ErrorHandling.cpp
    lib/LoggingInternal.cpp
    lib/LoggingNative.cpp
    lib/LoggingTesting.cpp)

add_library (Logging SHARED ${SOURCE_FILES})

add_dependencies (Logging prep)

target_include_directories (Logging PUBLIC
    include
    include/wil
    ${WOCStdLib_HEADERS})

target_compile_definitions (Logging PUBLIC OBJC_PORT)
