set(app_src app_mqtt.c)
set(pri_req driver esp_timer mqtt json)
idf_component_register(SRCS ${app_src} 
                    INCLUDE_DIRS "."
                    REQUIRES ${pri_req})