set(app_src app_config.c)
set(pri_req driver esp_timer esp_wifi nvs_flash)
idf_component_register(SRCS ${app_src}
                    INCLUDE_DIRS "."
                    REQUIRES ${pri_req})