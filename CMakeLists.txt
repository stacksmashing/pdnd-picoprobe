cmake_minimum_required(VERSION 3.12)

include(pico_sdk_import.cmake)

project(pdnd_picoprobe)

pico_sdk_init()

add_executable(pdnd_picoprobe)

pico_generate_pio_header(pdnd_picoprobe ${CMAKE_CURRENT_LIST_DIR}/src/pdnd/pio/spi.pio)
pico_generate_pio_header(pdnd_picoprobe ${CMAKE_CURRENT_LIST_DIR}/src/pdnd/pio/i2c.pio)
pico_generate_pio_header(pdnd_picoprobe ${CMAKE_CURRENT_LIST_DIR}/src/pdnd/pio/uart_rx.pio)
pico_generate_pio_header(pdnd_picoprobe ${CMAKE_CURRENT_LIST_DIR}/src/pdnd/pio/uart_tx.pio)

target_sources(pdnd_picoprobe PRIVATE
        src/led.c
        src/main.c
        src/usb_descriptors.c
        src/probe.c
        src/cdc_uart.c
        src/get_serial.c


        # PIO components
        src/pdnd/pio/pio_spi.c
        src/pdnd/pio/pio_i2c.c

        # PDND source
        src/pdnd/pdnd.c
        src/pdnd/pdnd_spi.c

        # Display support
        src/pdnd/pdnd_display.c
        src/pdnd/display/FreeSans5pt7b.c
        src/pdnd/display/FreeSans9pt7b.c
        src/pdnd/display/font_renderer.c
        src/pdnd/display/ssd1306.c
)

if (DEFINED ENV{PICOPROBE_LED})
        message("PICOPROBE_LED is defined as " $ENV{PICOPROBE_LED})
        target_compile_definitions(pdnd_picoprobe PRIVATE PICOPROBE_LED=$ENV{PICOPROBE_LED})
endif()

set(DBG_PIN_COUNT=4)

pico_generate_pio_header(pdnd_picoprobe ${CMAKE_CURRENT_LIST_DIR}/src/probe.pio)

target_include_directories(pdnd_picoprobe PRIVATE src)

target_link_libraries(pdnd_picoprobe PRIVATE pico_stdlib pico_unique_id tinyusb_device tinyusb_board hardware_pio)

pico_add_extra_outputs(pdnd_picoprobe)
