set(app_src http_server_app.c)
set(pri_req driver esp_wifi esp_eth esp_http_server)
idf_component_register(SRCS ${app_src} 
                    INCLUDE_DIRS "."
                    REQUIRES ${pri_req})