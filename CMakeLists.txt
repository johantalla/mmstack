cmake_minimum_required(VERSION 3.25)
project(mmstack LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)   # clang-tidy and your editor read this

# Warnings: one target everything links against
add_library(mm_warnings INTERFACE)
target_compile_options(mm_warnings INTERFACE
  -Wall -Wextra -Wpedantic -Wshadow -Wconversion -Wsign-conversion)

# Sanitizers, chosen per build preset
set(MM_SANITIZE "" CACHE STRING "e.g. address,undefined or thread")
if(MM_SANITIZE)
  add_compile_options(-fsanitize=${MM_SANITIZE} -fno-omit-frame-pointer -g)
  add_link_options(-fsanitize=${MM_SANITIZE})
endif()

add_executable(tsc_calibrate tools/tsc_calibrate.cpp)
target_link_libraries(tsc_calibrate PRIVATE mm_warnings)