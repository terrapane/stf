# Threading support is required
find_package(Threads REQUIRED)

# Include standard installation directory variables
include(GNUInstallDirs)

# Create the STF library and the alias for consistent usage with
# both installed an installed library and FetchContent
add_library(stf STATIC stf.cpp)
add_library(Terra::stf ALIAS stf)

# Specify the internal and public include directories
target_include_directories(stf
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

# Link against library dependencies
target_link_libraries(stf PRIVATE Threads::Threads)

# Specify the C++ standard to observe
message(STATUS "Building with ${stf_CPP_STD}")
set_target_properties(stf
    PROPERTIES
        CXX_STANDARD ${stf_CPP_STD}
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF)

# If requesting clang-tidy, try to look for it
if(stf_CLANG_TIDY)
    find_program(CLANG_TIDY_COMMAND NAMES "clang-tidy")
    if(CLANG_TIDY_COMMAND)
        set_target_properties(stf PROPERTIES CXX_CLANG_TIDY "${CLANG_TIDY_COMMAND}")
    else()
        message(WARNING "Could not find clang-tidy")
    endif()
endif()

# Use the following compile options
target_compile_options(stf
    PRIVATE
        $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>:-Wpedantic -Wextra -Wall -Werror>
        $<$<CXX_COMPILER_ID:MSVC>:/W4 /WX /Zc:__cplusplus>)

# Install target and associated include files
if(stf_INSTALL)
    install(TARGETS stf EXPORT stfTargets ARCHIVE)
    install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ TYPE INCLUDE)
    install(EXPORT stfTargets
            FILE stfConfig.cmake
            NAMESPACE Terra::
            DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stf)
endif()
