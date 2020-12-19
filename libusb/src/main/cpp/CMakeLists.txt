cmake_minimum_required(VERSION 3.6)
project(libusb VERSION 1.0.24 LANGUAGES C)

find_library(log-lib log)

set(SOURCE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/libusb")

set(USB_SOURCES
        "${SOURCE_PATH}/libusb/core.c" "${SOURCE_PATH}/libusb/descriptor.c"
        "${SOURCE_PATH}/libusb/hotplug.c" "${SOURCE_PATH}/libusb/io.c"
        "${SOURCE_PATH}/libusb/sync.c" "${SOURCE_PATH}/libusb/strerror.c"
        "${SOURCE_PATH}/libusb/os/linux_usbfs.c" "${SOURCE_PATH}/libusb/os/events_posix.c"
        "${SOURCE_PATH}/libusb/os/threads_posix.c" "${SOURCE_PATH}/libusb/os/linux_netlink.c")

add_library(usb1.0 SHARED ${USB_SOURCES})
set_target_properties(usb1.0 PROPERTIES OUTPUT_NAME usb1.0 EXPORT_NAME LibUSBShared)
target_include_directories(usb1.0 PRIVATE "${SOURCE_PATH}/libusb" "${SOURCE_PATH}/android" "${SOURCE_PATH}/libusb/os")
target_include_directories(usb1.0 PUBLIC
        $<BUILD_INTERFACE:${SOURCE_PATH}/libusb>
        $<INSTALL_INTERFACE:include/libusb>)
target_link_libraries(usb1.0 ${log-lib})
