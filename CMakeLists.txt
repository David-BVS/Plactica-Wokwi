cmake_minimum_required(VERSION 3.13)

# Pico SDK import (set PICO_SDK_PATH in your environment)
include(pico_sdk_import.cmake)

project(plactica_wokwi C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(plactica_wokwi
  src/main.cpp
)

# This project uses Arduino-style APIs (Keypad + pinMode/digitalWrite)
# and is intended for simulation/documentation-first workflows.
# Keep source logic unchanged.

pico_enable_stdio_uart(plactica_wokwi 1)
pico_enable_stdio_usb(plactica_wokwi 0)

pico_add_extra_outputs(plactica_wokwi)
