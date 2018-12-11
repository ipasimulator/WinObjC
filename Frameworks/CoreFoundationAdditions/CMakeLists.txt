set (SOURCE_FILES
    _NSCFNumber.mm
    _NSCFTemporaryRootObject.mm
    _NSCFString.mm
    _NSCFType.mm
    _CFLocaleInternal.mm
    CFStringTokenizer.mm)

add_library (CoreFoundationAdditions STATIC ${SOURCE_FILES})
add_prep_dep (CoreFoundationAdditions)

target_include_directories (CoreFoundationAdditions PUBLIC
    ../../include
    ../include
    ../include/xplat
    ../../include/xplat
    ../../tools/include/xplat
    # From `CoreFoundationAdditionsLib.vcxproj`.
    ../CoreFoundation/Base.subproj
    # TODO: Where does this come from?
    ../../tools/include
    ../../tools/deps/prebuilt/include) # Currently only for `dispatch`

target_link_libraries (CoreFoundationAdditions PUBLIC
    woc-Logging)

target_compile_options (CoreFoundationAdditions PUBLIC
    -fblocks
    -includeStubIncludes.h
    -includeWOCStdLib.h
    -includeCoreFoundation_Prefix.h
    # Warnings
    -Wno-nullability-completeness)

target_compile_definitions (CoreFoundationAdditions
    PRIVATE
        CF_BUILDING_CF=1
    PUBLIC
        NO_STUBS
        INCLUDE_OBJC=1
        DEPLOYMENT_TARGET_WINDOWS=1
        HAVE_STRUCT_TIMESPEC=1
        # TODO: Share with `Starboard/CMakeLists.txt`.
        DISABLE_WINRT_DEPRECATION)
