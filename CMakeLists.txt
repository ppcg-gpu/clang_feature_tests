cmake_minimum_required(VERSION 3.15)

project(clang_feature_tests LANGUAGES C CXX)

# Add custom modules path
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Find dependencies using our custom modules
find_package(LLVMClang REQUIRED)

# Include CMake modules
include(CheckCXXSourceCompiles)
include(CheckIncludeFileCXX)
include(CheckSymbolExists)

# Include our custom Clang feature detection module
include(CheckClangFeatures)

# Check for various Clang features
check_clang_features()

configure_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/clang_features.h.in"
  "${CMAKE_CURRENT_BINARY_DIR}/clang_features.h"
@ONLY
)

message(STATUS "Generated clang_features.h for target ${TARGET}")

add_library(${PROJECT_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# Add the include directory to the target
target_include_directories(${PROJECT_NAME} INTERFACE "${CMAKE_CURRENT_BINARY_DIR}")

# Set up include directories and definitions for LLVM/Clang
target_include_directories(${PROJECT_NAME} INTERFACE
    ${LLVM_INCLUDE_DIRS} ${CLANG_INCLUDE_DIRS})
add_definitions(${PROJECT_NAME} INTERFACE ${LLVM_DEFINITIONS})
target_link_directories(${PROJECT_NAME} INTERFACE ${LLVM_LIBRARY_DIRS})

# Link Clang/LLVM libraries
target_link_libraries(${PROJECT_NAME} INTERFACE ${CLANG_LIBRARIES} ${LLVM_LIBRARIES})
