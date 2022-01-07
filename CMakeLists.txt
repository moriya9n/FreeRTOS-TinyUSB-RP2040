cmake_minimum_required(VERSION 3.13)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(example C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(main_blinky
        src/main.c
        src/main_blinky.c
        src/freertos_hook.c
        src/usb_descriptors.c
        tinyusb/src/tusb.c
        tinyusb/src/common/tusb_fifo.c
        tinyusb/src/device/usbd.c
        tinyusb/src/device/usbd_control.c
        tinyusb/src/class/cdc/cdc_device.c
        tinyusb/src/portable/raspberrypi/rp2040/rp2040_usb.c
        tinyusb/src/portable/raspberrypi/rp2040/dcd_rp2040.c
        FreeRTOS-Kernel/tasks.c
        FreeRTOS-Kernel/queue.c
        FreeRTOS-Kernel/list.c
        FreeRTOS-Kernel/timers.c
        FreeRTOS-Kernel/tasks.c
        FreeRTOS-Kernel/event_groups.c
        FreeRTOS-Kernel/portable/ThirdParty/GCC/RP2040/port.c
        FreeRTOS-Kernel/portable/MemMang/heap_1.c
        )

add_subdirectory(
        FreeRTOS-Kernel/portable/ThirdParty/GCC/RP2040
)

target_compile_definitions(main_blinky PRIVATE
        CFG_TUSB_MCU=OPT_MCU_RP2040
        CFG_TUSB_OS=OPT_OS_FREERTOS
        )

target_include_directories(main_blinky PRIVATE
        src
        tinyusb/hw
        tinyusb/src
        tinyusb/src/class/cdc
        FreeRTOS-Kernel/include
        FreeRTOS-Kernel/portable/ThirdParty/GCC/RP2040/include
        ${PICO_SDK_PATH}/src/rp2_common/hardware_exception/include
)

target_link_libraries(main_blinky pico_stdlib hardware_exception)
pico_add_extra_outputs(main_blinky)

