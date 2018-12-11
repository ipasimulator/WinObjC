# Note that this is actually included from the root `CMakeLists.txt` as well as
# from the parent directory. The difference is that once `CL_COMPILER` is set to
# `OFF` and the other time to `ON`, respectively.

add_subdirectory (CoreFoundation)
add_subdirectory (CoreFoundationAdditions)
add_subdirectory (Starboard)
