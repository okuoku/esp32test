cmake_minimum_required(VERSION 3.14) # For RULE_LAUNCH_CUSTOM fix

set(idf_path ${CMAKE_CURRENT_LIST_DIR}/external/esp-idf)
set(ENV{IDF_PATH} ${idf_path})

# Generate custom launcher
set(launcher_file ${CMAKE_CURRENT_BINARY_DIR}/runtool.sh)
set(launcher_src "#!/bin/sh\nexport IDF_PATH=${idf_path}\nexec $*\n")
file(WRITE ${launcher_file} ${launcher_src})
set_property(GLOBAL
    PROPERTY RULE_LAUNCH_CUSTOM
    ${launcher_file})

execute_process(COMMAND
    chmod +x ${launcher_file})

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(esp32test)
