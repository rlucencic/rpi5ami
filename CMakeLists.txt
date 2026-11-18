# sdlami: the Amiga 500, 2000, 600 and 1200 with SDL 3.
#
#   cmake -S . -B build -G Ninja && cmake --build build
#   build/sdlami --help
#
# The Amiga itself is esp32ami's (../esp32ami/components/amiga, platform
# independent), the runtime sdlemu's (../sdlemu), the 68000 and the
# 68EC020 (Musashi) esp32emu's (../esp32emu): all four cloned side by side.
cmake_minimum_required(VERSION 3.20)
project(sdlami C)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "" FORCE)
endif()

set(ESP32AMI_DIR "${CMAKE_CURRENT_LIST_DIR}/../esp32ami" CACHE PATH "esp32ami checkout: the Amiga")
if(NOT EXISTS "${ESP32AMI_DIR}/components/amiga/include/amiga.h")
    message(FATAL_ERROR "No esp32ami at ${ESP32AMI_DIR}: clone github.com/rlucencic/esp32ami beside sdlami")
endif()

add_subdirectory("${CMAKE_CURRENT_LIST_DIR}/../sdlemu" sdlemu)
find_package(ZLIB REQUIRED)         # RP9 packages (the boards use miniz)

file(GLOB AMI_SOURCES "${ESP32AMI_DIR}/components/amiga/*.c")
add_executable(sdlami
    src/main.c
    src/settings.c
    src/rom_store.c
    ${AMI_SOURCES})
target_include_directories(sdlami PRIVATE
    src
    "${ESP32AMI_DIR}/components/amiga/include"
    "${ESP32AMI_DIR}/components/amiga")
# Musashi as esp32ami's: the CPU (68000 or 68EC020) chosen at run time, no
# bus errors, instruction reads apart (the 68EC020's cache), the prefetch
target_compile_definitions(sdlami PRIVATE M68K_RUNTIME M68K_NO_BUS_ERRORS M68K_PROGRAM_READS M68K_PREFETCH)
sdlemu_add_cores(sdlami musashi)
# The web page's converters: DMS archives and gzipped images unpacked in the browser
sdlemu_embed(sdlami "${ESP32AMI_DIR}/main/web/amifiles.js" amifiles_js)
target_link_libraries(sdlami PRIVATE sdlemu ZLIB::ZLIB)
target_compile_options(sdlami PRIVATE -Wall -Wextra -Wno-unused-parameter -Wno-unused-result)

install(TARGETS sdlami)
