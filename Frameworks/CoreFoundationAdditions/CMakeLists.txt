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
    ../../tools/include/xplat)
