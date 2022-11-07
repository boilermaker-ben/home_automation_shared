# Example

The following is an example YAML configuration for the ifan component


```yaml
substitutions:
  # https://esphome.io/guides/configuration-types.html#substitutions
  device_name: office_ceiling
  device_ap: office-ceiling-node
  device_description: Sonoff iFan04-L

esphome:
  # https://esphome.io/components/esphome
  name: ${device_name}
  comment: ${device_description}
  platform: ESP8266
  board: esp01_1m
  includes:
    - custom_components/ifan/ifan.h

wifi:
  # https://esphome.io/components/wifi
  ssid: !secret iot_ssid
  password: !secret iot_pass
  manual_ip:
    static_ip: !secret office_ceiling_ip
    gateway: !secret wifi_gateway
    subnet: !secret wifi_subnet
  ap:
    ssid: ${device_ap}
    password: "password"
    manual_ip:
      static_ip: !secret wifi_ap_ip
      gateway: !secret wifi_ap_gateway
      subnet: !secret wifi_ap_subnet
      dns1: !secret wifi_ap_dns1
      dns2: !secret wifi_ap_dns2

captive_portal:
  # https://esphome.io/components/captive_portal.html

api:
  # https://esphome.io/components/api
  
ota:
  # https://esphome.io/components/ota
  
web_server:
  port: 80
  # https://esphome.io/components/web_server.html

time:
  # https://esphome.io/components/time.html
  - platform: homeassistant
    id: homeassistant_time

text_sensor:
  # https://esphome.io/components/text_sensor/custom.html
  - platform: version
    # https://esphome.io/components/text_sensor/version.html
    name: ${device_name}_esphome_ver
  - platform: wifi_info
    # https://esphome.io/components/text_sensor/wifi_info.html
    ip_address:
      name: ${device_name}_IP
    ssid:
      name: ${device_name}_SSID
    bssid:
      name: ${device_name}_BSSID

# logger:
#   baud_rate: 9600
#   # https://esphome.io/components/logger

uart:
  # https://esphome.io/custom/uart.html
  id: ${device_name}_uart
  tx_pin: GPIO01
  rx_pin: GPIO03
  baud_rate: 9600

sensor:
  - platform: uptime
    # https://esphome.io/components/sensor/uptime.html
    name: ${device_name}_uptime
  - platform: wifi_signal
    # https://esphome.io/components/sensor/wifi_signal.html
    name: ${device_name}_wifi_signal
    update_interval: 60s

switch:
  - platform: restart
    # https://esphome.io/components/switch/restart.html
    name: ${device_name}_restart

button:
  # https://esphome.io/components/button/index.html
  - platform: template
    name: ${device_name}_cycle_fan
    on_press:
      then:
        - fan.cycle_speed: ${device_name}_fan
        
output:
  # https://esphome.io/components/output
  - platform: esp8266_pwm
    id: led_pin
    pin: GPIO13
    inverted: true

light:
  # https://esphome.io/components/light/binary.html
  - platform: monochromatic
    id: led1
    output: led_pin
    default_transition_length: 0s
    restore_mode: always off
  - platform: ifan
    id: ${device_name}_fan_light
    name: ${device_name}_fan_light
    
fan:
  # https://esphome.io/components/fan/index.html
  - platform: ifan
    id: ${device_name}_fan
    name: ${device_name}_fan
    # optional: allows you to disable the buzzer, enabled by default. 
    # buzzer_enable: false
```
