if (NOT CL_COMPILER)
    set (SOURCE_FILES
        ABAddressBook.mm
        ABGroup.mm
        ABMultiValue.mm
        ABMutableMultiValue.mm
        ABPerson.mm
        ABRecord.mm
        ABSource.mm
        ABAddressBookManagerInternal.mm
        ABRecordInternal.mm
        ABContactInternal.mm
        ABSourceInternal.mm
        ABMultiValueInternal.mm)

    # For `ObjCUWP*.lib`s specified in source files.
    link_directories (AddressBook PRIVATE
        "../../deps/prebuilt/Universal Windows/x86")

    add_library (AddressBook SHARED ${SOURCE_FILES})
    add_prep_dep (AddressBook)

    target_include_directories (AddressBook PRIVATE ${WOCFX_INCLUDE_DIRS})

    target_compile_definitions (AddressBook PRIVATE
        ${WINOBJC_DEFS}
        # From `AddressBookLib.vcxproj`.
        STARBOARD_PORT=1
        DADDRESSBOOK_IMPEXP=)

    target_compile_options (AddressBook PRIVATE
        ${WINOBJC_CLANG_OPTIONS}
        # From `AddressBookLib.vcxproj`.
        "${OBJC_ARC}")

    target_link_libraries (AddressBook PRIVATE
        ${WOCFX_LIBS}
        # From `AddressBook.vcxproj`.
        "-Wl,/def:${CMAKE_CURRENT_SOURCE_DIR}/../../build/AddressBook/dll/\
AddressBook.def")
endif (NOT CL_COMPILER)
