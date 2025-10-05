cmake_minimum_required(VERSION 3.13)

# Import the Pico SDK
include(pico_sdk_import.cmake)

project(nrf_node C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the Pico SDK
pico_sdk_init()

# Add subdirectory for SSD1306 library (uncomment later)
add_subdirectory(lib/pico-ssd1306)

target_include_directories(pico_ssd1306 PUBLIC
    ${PICO_SDK_PATH}/src/rp2_common/hardware_gpio/include
    ${PICO_SDK_PATH}/src/rp2_common/hardware_i2c/include
    ${PICO_SDK_PATH}/src/common/pico_stdlib/include
    ${PICO_SDK_PATH}/src/rp2_common/hardware_platform/include
    ${PICO_SDK_PATH}/src/rp2_common/hardware_sync/include
    ${PICO_SDK_PATH}/src/rp2_common/pico_base/include
    ${PICO_SDK_PATH}/src/rp2_common/hardware_regs/include
    ${PICO_SDK_PATH}/src/rp2_common/hardware_structs/include
)

# Create main executable
add_executable(nrf_node
    src/main.c
)

# Link your libraries
target_link_libraries(nrf_node
    pico_stdlib
    hardware_i2c
    pico_ssd1306  # Uncomment when ready
)

# Enable USB or UART stdio
pico_enable_stdio_usb(nrf_node 1)
pico_enable_stdio_uart(nrf_node 0)

# Generate UF2 and other outputs
pico_add_extra_outputs(nrf_node)
