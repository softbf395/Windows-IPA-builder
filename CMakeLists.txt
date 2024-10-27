# Minimum CMake version requirement
cmake_minimum_required(VERSION 3.15)

# Project name and version
project(MyIOSApp VERSION 1.0)

# Specify CMake system as iOS
set(CMAKE_SYSTEM_NAME iOS)
set(CMAKE_OSX_ARCHITECTURES "arm64")  # Target arm64 for iOS devices
set(CMAKE_OSX_DEPLOYMENT_TARGET "12.0")  # Minimum iOS version

# Get all files in the src/iOS directory, excluding Info.plist
file(GLOB_RECURSE SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/iOS/*"
)

# Remove Info.plist from SOURCES if it exists
list(REMOVE_ITEM SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/iOS/Info.plist")

# Add executable with the found sources
add_executable(MyIOSApp ${SOURCES})

# Set target properties for creating an iOS bundle (.app)
set_target_properties(MyIOSApp PROPERTIES
    MACOSX_BUNDLE TRUE
    MACOSX_BUNDLE_BUNDLE_NAME "MyIOSApp"
    MACOSX_BUNDLE_IDENTIFIER "com.example.MyIOSApp"
    MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/src/iOS/Info.plist"
)

# Link necessary iOS frameworks (UIKit and Foundation)
find_library(UIKIT UIKit)
find_library(FOUNDATION Foundation)
target_link_libraries(MyIOSApp ${UIKIT} ${FOUNDATION})

# Specify output directory to structure .app package in Payload for IPA creation
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Payload/MyIOSApp.app)
