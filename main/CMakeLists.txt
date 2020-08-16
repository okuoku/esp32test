idf_component_register(
    SRCS "hello_world_main.c"
    REQUIRES 
    LovyanGFX 
    spi_flash # For hello world sample
    INCLUDE_DIRS "")
