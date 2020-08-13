cmake_minimum_required(VERSION 3.14) # For RULE_LAUNCH_CUSTOM fix

set(PORT /dev/ttyS4)
set(idf_path ${CMAKE_CURRENT_LIST_DIR}/external/esp-idf)
set(ENV{IDF_PATH} ${idf_path})

# Generate custom launcher
set(launcher_file ${CMAKE_CURRENT_BINARY_DIR}/runtool.sh)
set(launcher_src "#!/bin/sh\nexport ESPPORT=${PORT}\nexport IDF_PATH=${idf_path}\nexec \"$@\"\n")
file(WRITE ${launcher_file} ${launcher_src})
set_property(GLOBAL
    PROPERTY RULE_LAUNCH_CUSTOM
    ${launcher_file})

execute_process(COMMAND
    chmod +x ${launcher_file})


set(EXTRA_COMPONENT_DIRS 
    # ESP32 components
    #${CMAKE_CURRENT_LIST_DIR}/external/arduino

    # App 
    ${CMAKE_CURRENT_LIST_DIR}/arduino/LovyanGFX
    #${CMAKE_CURRENT_LIST_DIR}/arduino/ESP32-Chimera-Core
    )

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(esp32test)

# Custom projects for tools

add_custom_target(mon
    COMMAND
    ${PYTHON} ${idf_path}/tools/idf_monitor.py
    --port ${PORT}
    --baud 115200
    esp32test.elf
    USES_TERMINAL)

