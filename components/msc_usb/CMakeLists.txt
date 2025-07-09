idf_component_register(SRCS "msc_usb.c"
                    INCLUDE_DIRS "include"
                    REQUIRES  usb fatfs esp_driver_gpio esp_timer espressif__usb_host_msc)
