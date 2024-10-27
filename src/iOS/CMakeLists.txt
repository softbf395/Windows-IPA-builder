# Minimum CMake version requirement
cmake_minimum_required(VERSION 3.15)

# Project name and version
project(MyIOSApp VERSION 1.0)

# Specify CMake system as iOS
set(CMAKE_SYSTEM_NAME iOS)
set(CMAKE_OSX_ARCHITECTURES "arm64")  # Target arm64 for iOS devices
set(CMAKE_OSX_DEPLOYMENT_TARGET "12.0")  # Minimum iOS version

# Get all specified file types from the parent directory of src/iOS
file(GLOB_RECURSE SOURCE_FILES
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.png"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.jpeg"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.webp"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.mp3"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.ogg"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.json"
    "${CMAKE_CURRENT_SOURCE_DIR}/../*.plist"
)

# Create an executable target for the iOS app
add_executable(MyIOSApp ${SOURCE_FILES})

# Set target properties for creating an iOS bundle (.app)
set_target_properties(MyIOSApp PROPERTIES
    MACOSX_BUNDLE TRUE
    MACOSX_BUNDLE_BUNDLE_NAME "MyIOSApp"
    MACOSX_BUNDLE_IDENTIFIER "com.example.MyIOSApp"
    MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/Info.plist"
)

# Link necessary iOS frameworks (UIKit and Foundation)
find_library(UIKIT UIKit)
find_library(FOUNDATION Foundation)
target_link_libraries(MyIOSApp ${UIKIT} ${FOUNDATION})

# Specify output directory to structure .app package in Payload for IPA creation
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Payload/MyIOSApp.app)
