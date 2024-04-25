# Arlec GLD122HA ESPHome

This is a screw-type Edison E27 RGBWW smart-bulb available from Bunnings in Australia and New Xealand

It contains a Tuya TYWE3L module with an ESP8266 chip. Datasheet for the module:
[https://developer.tuya.com/en/docs/iot/wb2l-datasheet?id=K9duegc9bualu](https://developer.tuya.com/en/docs/iot/wifie3lpinmodule?id=K9605uj1ar87n)

Here are the current measurements with the stock firmware:
| Colour | Current |
| ------ | ------- |
| Whites | 37mA    |
| Red    | 11mA    |
| Green  | 12mA    |
| Blue   | 13mA    |

Unfortunately Tuya-cloudcutter doesn't work on this model. You'll need to crack it open and flash it with a USB-TTL adaptor. Heat around where the white plastic meets the diffuse plastic then separate it with a spudger. Then pry up the LED board with a pick.

![PXL_20240422_071536157-1](https://github.com/neilbags/arlec-grid-gld122ha-esphome/assets/2738833/b7d688bd-88a4-4afa-80d1-9ff4dcfe5be5)

These are the pins you need to connect to flash the module.
| Wire Color | Function | Location | Notes |
| ---------- | -------- | -------- | ----- |
| Green      | TX       | First pin on the left | Connect to RX on your USB-TTL Adaptor |
| Blue       | RX       | Second pin on the left | Connect to TX on your USB-TTL Adaptor |
| White-Orange | GPIO0 | Fifth pin on the left | Connect to GND during flashing |
| White-Blue | 3.3v | Positive side of small capacitor | Connect to 3.3v power supply
| White-Green | GND | Negative side of large capacitor (underneath) | Connect to GND on power supply and USB-TTL adaptor |

![PXL_20240422_071550444-1](https://github.com/neilbags/arlec-grid-gld122ha-esphome/assets/2738833/d880f516-508f-4248-b189-88b9ca64ca1a)

Now visit https://web.esphome.io/ to flash the basic ESPHome firmware. It will reboot when done but won't come back up unless you disconnect GPIO from GND.

Here's the working config:
```yaml
esphome:
  name: arlec4
  friendly_name: arlec4

esp8266:
  board: esp01_1m

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: <your-key>

ota:
  password: <your-password>

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Arlec4 Fallback Hotspot"
    password: <your password>

captive_portal:

light:
  - platform: rgbww
    name: "light1"
    red: red
    green: green
    blue: blue
    cold_white: cool
    warm_white: warm
    cold_white_color_temperature: 6536 K
    warm_white_color_temperature: 2000 K
    color_interlock: true

output:
  - platform: esp8266_pwm
    id: cool
    pin: GPIO5
    max_power: 0.85
  - platform: esp8266_pwm
    id: warm
    pin: GPIO13
    max_power: 0.85
  - platform: esp8266_pwm
    id: red
    pin: GPIO4
    max_power: 0.7
  - platform: esp8266_pwm
    id: green
    pin: GPIO12
    max_power: 0.8
  - platform: esp8266_pwm
    id: blue
    pin: GPIO14
    max_power: 0.8
```
