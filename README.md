This is a screw-type Edison E27 RGBWW smart-bulb available from Bunnings

It contains a Tuya TYWE3L module with an ESP8266 chip. Here is the datasheel for the module:
[https://developer.tuya.com/en/docs/iot/wb2l-datasheet?id=K9duegc9bualu](https://developer.tuya.com/en/docs/iot/wifie3lpinmodule?id=K9605uj1ar87n)

Here are the current measurements with the stock firmware:
| Colour | Current |
| ------ | ------- |
| Whites | 37mA    |
| Red    | 11mA    |
| Green  | 12mA    |
| Blue   | 13mA    |

Tuya-cloudcutter doesn't work on this model. You'll need to crack it open and flash it with a USB-TTL adaptor. Heat around where the white plastic meets the diffuse plastic then separate it with a spudger. Then pry up the LED board with a pick.

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

Configuration coming soon
