idf_component_register(SRCS "app_main.c"
                            "../common/http_server_app/http_server_app.c"
                            "../common/app_config/app_config.c"
                            "../common/mqtt/app_mqtt.c"             
                    INCLUDE_DIRS "."
                            "../common/http_server_app"
                            "../common/app_config"
                            "../common/mqtt"
                    REQUIRES mqtt json esp_wifi nvs_flash esp_http_server driver esp_timer)
target_compile_options(${COMPONENT_LIB} PRIVATE "-Wno-format")



