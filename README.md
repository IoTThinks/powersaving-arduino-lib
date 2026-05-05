# PowerSaving-arduino-lib
* Releases: https://github.com/IoTThinks/powersaving-arduino-lib/releases
* Powered by [IoTThinks.com](https://iotthinks.com/) (8+ year in IoT / LoRa / LoRaWAN)

## PowerSaving Arduino Core 2.0.17 for low power ESP32 boards
PowerSaving Arduino Core 2.0.17 is built for low power ESP32 boards. 
- ESP32 boards with **active BLE** can reduced from 120mA down to **15-25mA**.
- Based on Arduino Core 2.0.17 and IDF 4.4.7
- Power Management enabled
- BLE Sleep enabled
- Supports ESP32, ESP32S3 and ESP32C3. 
- ESP32S2 is not supported yet due to low usage. If you need it, contact us at https://iotthinks.com/

## Support Us
* Enjoying our work? 
* Support us via [![Sponsor](https://img.shields.io/badge/Paypal-IoTThinks-0070ba?style=flat&logo=paypal&logoColor=white&labelColor=0070ba&color=555555)](https://www.paypal.com/ncp/payment/6FGWJJVC572VL) or [![Sponsor](https://img.shields.io/badge/GitHub%20Sponsors-IoTThinks-ea4aaa?style=flat&logo=github-sponsors&logoColor=white&labelColor=ea4aaa&color=555555)](https://github.com/sponsors/IoTThinks).

## Professional Services
* If you want custom development for your projects, [Contact Us](https://iotthinks.com/contact-us/)

## Instruction:
- In platformio.ini, use the below in your 
`platform_packages = platformio/framework-arduinoespressif32@https://github.com/IoTThinks/powersaving-arduino-lib/releases/download/2.0.17/esp32-2.0.17.zip`
- In main.cpp, include the libraries
```
#include "esp_pm.h"
#include "esp_bt.h"
```

- In setup(), enable PowerSaving mode

```
// Enable BLE sleep
  esp_err_t errBLESleep = esp_bt_sleep_enable();
  if (errBLESleep == ESP_OK) {
    Serial.println("Bluetooth sleep enabled successfully");
  } else {
    Serial.printf("Bluetooth sleep enable failed: %s\n", esp_err_to_name(errBLESleep));
  }

#if CONFIG_IDF_TARGET_ESP32C3
  esp_pm_config_esp32c3_t pm_config;
#elif CONFIG_IDF_TARGET_ESP32S3
  esp_pm_config_esp32s3_t pm_config;
#elif CONFIG_IDF_TARGET_ESP32
  esp_pm_config_esp32_t pm_config;
#endif

  // Configure Power Management
  pm_config = { .max_freq_mhz = 80, .min_freq_mhz = 40, .light_sleep_enable = true };
  esp_err_t errPM = esp_pm_configure(&pm_config);
  if (errPM == ESP_OK) {
    Serial.println("Power Management configured successfully");
  } else {
    Serial.printf("Power Management failed to configure: %d\r\n", errPM);
  }
```

- In loop, make some vTaskDelay() to let MCU have some time to sleep

`vTaskDelay(pdMS_TO_TICKS(50)); // attempt to sleep`

## Demo
* Default PowerSaving for ESP32 companion BLE. Reduced from 120mA down to 15-25mA.
<img height="384" alt="image" src="https://github.com/user-attachments/assets/bcae8449-4646-4a98-852f-312582686ad6" />
<img height="384" alt="image" src="https://github.com/user-attachments/assets/2fc5bee1-fe89-4c8f-8097-2db181d89f4a" />
<img height="384" alt="image" src="https://github.com/user-attachments/assets/656fcd90-3211-4044-832b-a63f30c2fb4a" />
<img height="384" alt="image" src="https://github.com/user-attachments/assets/7611ebb5-d2d1-42b7-8845-6016e838b0cf" />
