set (SOURCE_FILES
    lib/ErrorHandling.cpp
    lib/LoggingInternal.cpp
    lib/LoggingNative.cpp
    lib/LoggingTesting.cpp
    dll/Telemetry.cpp)

add_library (Logging SHARED ${SOURCE_FILES})

add_prep_dep (Logging)

target_include_directories (Logging PUBLIC
    include
    include/wil
    ../include/WOCStdLib)

target_link_libraries (Logging PUBLIC AppInsights
    PRIVATE "-def:${CMAKE_CURRENT_SOURCE_DIR}/dll/Logging.def")

target_compile_options (Logging PUBLIC /ZW)
