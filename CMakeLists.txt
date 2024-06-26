cmake_minimum_required(VERSION 3.13)

# initialize pico_sdk from GIT
# (note this can come from environment, CMake cache etc)
# set(PICO_SDK_FETCH_FROM_GIT on)

# pico_sdk_import.cmake is a single file copied from this SDK
# note: this must happen before project()
include(pico_sdk_import.cmake)

project(pico_rmii_ethernet)

# initialize the Pico SDK
pico_sdk_init()

add_library(pico_rmii_ethernet INTERFACE)

target_sources(pico_rmii_ethernet INTERFACE
    ${CMAKE_CURRENT_LIST_DIR}/src/rmii_ethernet.c
)

target_include_directories(pico_rmii_ethernet INTERFACE
	${CMAKE_CURRENT_LIST_DIR}/src/include
	${CMAKE_CURRENT_LIST_DIR}/src/lwip
)

pico_generate_pio_header(pico_rmii_ethernet ${CMAKE_CURRENT_LIST_DIR}/src/rmii_ethernet_phy_rx.pio)
pico_generate_pio_header(pico_rmii_ethernet ${CMAKE_CURRENT_LIST_DIR}/src/rmii_ethernet_phy_tx.pio)

target_link_libraries(pico_rmii_ethernet INTERFACE hardware_pio hardware_dma pico_stdlib pico_unique_id  pico_lwip_nosys pico_lwip_http)

add_subdirectory("examples/httpd")
