# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
include(${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
# ====================================================================================
set(PICO_BOARD pico2 CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(picocd C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(picocd
    src/main.cc
    src/usb.c

    src/printf/printf.c
    src/printf/putchar.c
    
    src/roms/spelunk-en.rom.cc
    src/roms/pwnd.rom.cc
    )

pico_generate_pio_header(picocd ${CMAKE_CURRENT_SOURCE_DIR}/src/bus.pio)

# Make sure TinyUSB can find tusb_config.h
target_include_directories(picocd PUBLIC ${CMAKE_CURRENT_LIST_DIR}/src)

# pull in common dependencies
target_link_libraries(picocd 
#    pico_stdlib 
    hardware_gpio
#    hardware_uart
    hardware_divider
    pico_time
    pico_util
    pico_platform
    pico_runtime
#    pico_stdio

    hardware_pio 
    pico_multicore
    tinyusb_host 
    tinyusb_board)


#if (PICO_CYW43_SUPPORTED)
#    target_link_libraries(blink pico_cyw43_arch_none)
#endif()

# create map/bin/hex file etc.
pico_add_extra_outputs(picocd)

set (PICO_COPY_TO_RAM 1)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0")

# add url via pico_set_program_url

# family_configure_host_example(picocd noos)
