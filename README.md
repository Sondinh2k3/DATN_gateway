# Gateway ESP32 - Hệ Thống Giám Sát Mực Nước và Vị Trí

## Tổng Quan
Đây là dự án Gateway ESP32 được thiết kế để thu thập và truyền dữ liệu từ các cảm biến mực nước và GPS thông qua giao thức LoRa, sau đó gửi dữ liệu lên MQTT broker để xử lý và hiển thị.

## Tính Năng Chính
- Kết nối WiFi và MQTT broker
- Thu thập dữ liệu mực nước qua LoRa mỗi 5 giây
- Thu thập dữ liệu GPS (kinh độ, vĩ độ) qua LoRa mỗi 1 giây
- Gửi dữ liệu lên MQTT broker dưới dạng JSON
- Giao diện web để giám sát và điều khiển

## Cấu Trúc Dự Án
```
gateway/
├── common/
│   ├── app_config/        # Cấu hình WiFi và các thông số hệ thống
│   ├── http_server_app/   # Giao diện web server
│   ├── lora/             # Xử lý giao tiếp LoRa
│   ├── mqtt/             # Xử lý giao tiếp MQTT
│   └── sx1278_ra02/      # Thư viện driver LoRa
├── main/                 # Chương trình chính
├── html/                # File HTML cho web interface dùng để cấu hình wifi
└── build/              # Thư mục build
```

## Yêu Cầu Phần Cứng
- ESP32 Development Board
- Module LoRa SX1278 RA-02
- Kết nối WiFi
- Nguồn điện 5V

## Yêu Cầu Phần Mềm
- ESP-IDF (Espressif IoT Development Framework)
- MQTT Broker (ví dụ: Mosquitto)
- Python 3.x (để chạy các script hỗ trợ)

## Cài Đặt và Build
1. Cài đặt ESP-IDF theo hướng dẫn chính thức
2. Clone repository:
   ```bash
   git clone [repository_url]
   cd gateway
   ```
3. Cấu hình project:
   ```bash
   idf.py menuconfig
   ```
4. Build project:
   ```bash
   idf.py build
   ```
5. Flash lên ESP32:
   ```bash
   idf.py -p [PORT] flash monitor
   ```

## Cấu Hình
### WiFi
- SSID và password được cấu hình trong `app_config.h`
- Có thể thay đổi thông qua menuconfig

### MQTT
- Broker address: mqtt://192.168.1.11:1883 chạy Local trong máy (xem đúng ip trên máy bằng cách vào CMD và gõ: ipconfig)
- Topic: datn_gpsrtk
- Format dữ liệu JSON:
  ```json
  {
    "waterLevel": 1.5,
    "latitude": 10.762622,
    "longitude": 106.660172
  }
  ```

### LoRa
- Tần số: 433MHz
- Bandwidth: 125kHz
- Spreading Factor: 7
- Coding Rate: 5
- Preamble Length: 8
- Sync Word: 0x12

## Định Dạng Dữ Liệu LoRa
### Mực Nước
- Format: "WATER:XX.XX"
- Ví dụ: "WATER:1.50"
- Tần suất: 5 giây/lần

### GPS
- Format: "GPS:XX.XXXXX,YY.YYYYY"
- Ví dụ: "GPS:10.76262,106.66017"
- Tần suất: 1 giây/lần

## Sử Dụng
1. Kết nối phần cứng theo sơ đồ
2. Cấu hình WiFi và MQTT broker
3. Flash firmware lên ESP32
4. Kiểm tra kết nối qua serial monitor
5. Truy cập web interface qua địa chỉ IP của ESP32

## Xử Lý Lỗi
- Kiểm tra kết nối WiFi
- Kiểm tra kết nối MQTT broker
- Kiểm tra tín hiệu LoRa
- Xem log qua serial monitor

## Đóng Góp
Mọi đóng góp đều được hoan nghênh. Vui lòng tạo issue hoặc pull request.

## Giấy Phép
Dự án này được phát hành dưới giấy phép MIT. Xem file LICENSE để biết thêm chi tiết.

| Supported Targets | ESP32 | ESP32-C3 | ESP32-S2 | ESP32-S3 |
| ----------------- | ----- | -------- | -------- | -------- |

# ESP-MQTT SSL Sample application (mutual authentication)

(See the README.md file in the upper level 'examples' directory for more information about examples.)

This example connects to the broker test.mosquitto.org using ssl transport with client certificate and as a demonstration subscribes/unsubscribes and send a message on certain topic.
(Please note that the public broker is maintained by the community so may not be always available, for details please visit http://test.mosquitto.org)

It uses ESP-MQTT library which implements mqtt client to connect to mqtt broker.

## How to use example

### Hardware Required

This example can be executed on any ESP32 board, the only required interface is WiFi and connection to internet.

### Configure the project

* Open the project configuration menu (`idf.py menuconfig`)
* Configure Wi-Fi or Ethernet under "Example Connection Configuration" menu. See "Establishing Wi-Fi or Ethernet Connection" section in [examples/protocols/README.md](../../README.md) for more details.

* Generate your client keys and certificate

Navigate to the main directory

```
cd main
```

Generate a client key and a CSR. When you are generating the CSR, do not use the default values. At a minimum, the CSR must include the Country, Organisation and Common Name fields.

```
openssl genrsa -out client.key
openssl req -out client.csr -key client.key -new
```

Paste the generated CSR in the [Mosquitto test certificate signer](https://test.mosquitto.org/ssl/index.php), click Submit and copy the downloaded `client.crt` in the `main` directory.

Please note, that the supplied files `client.crt` and `client.key` in the `main` directory are only placeholders for your client certificate and key (i.e. the example "as is" would compile but would not connect to the broker)

The server certificate `mosquitto.org.crt` can be downloaded in pem format from [mosquitto.org.crt](https://test.mosquitto.org/ssl/mosquitto.org.crt).

### Build and Flash

Build the project and flash it to the board, then run monitor tool to view serial output:

```
idf.py -p PORT flash monitor
```

(To exit the serial monitor, type ``Ctrl-]``.)

See the Getting Started Guide for full steps to configure and use ESP-IDF to build projects.

## Example Output

```
I (3714) event: sta ip: 192.168.0.139, mask: 255.255.255.0, gw: 192.168.0.2
I (3714) system_api: Base MAC address is not set, read default base MAC address from BLK0 of EFUSE
I (3964) MQTT_CLIENT: Sending MQTT CONNECT message, type: 1, id: 0000
I (4164) MQTTS_EXAMPLE: MQTT_EVENT_CONNECTED
I (4174) MQTTS_EXAMPLE: sent publish successful, msg_id=41464
I (4174) MQTTS_EXAMPLE: sent subscribe successful, msg_id=17886
I (4174) MQTTS_EXAMPLE: sent subscribe successful, msg_id=42970
I (4184) MQTTS_EXAMPLE: sent unsubscribe successful, msg_id=50241
I (4314) MQTTS_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=41464
I (4484) MQTTS_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=17886
I (4484) MQTTS_EXAMPLE: sent publish successful, msg_id=0
I (4684) MQTTS_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=42970
I (4684) MQTTS_EXAMPLE: sent publish successful, msg_id=0
I (4884) MQTT_CLIENT: deliver_publish, message_length_read=19, message_length=19
I (4884) MQTTS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (5194) MQTT_CLIENT: deliver_publish, message_length_read=19, message_length=19
I (5194) MQTTS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```

