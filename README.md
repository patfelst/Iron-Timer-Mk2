# Iron-Timer-Mk2

![prototype photo](/Prototype_photo.jpg "Prototype")

## ESPHome based Iron Timer
Never forget to turn your clothes iron off again! Plug your clothes iron into a WiFi power switch which you've previously integrated into Home Assistant (HA). A rotary encoder allows you to input countdown time in minutes and seconds. Press the encoder button down, and the timer starts counting down. When it reaches zero, it instructs HA to switch off the iron. A 14-segment alphanumeric LED display shows the selected and remaining time.

![cct_diagram](/Circuit_diagram_Iron_Timer.svg "Iron timer circuit diagram")

### Parts List

------------

#### Rotary Encoder
Choice of "Non Powered" (3 examples below) **or** "Powered" (2 examples below)
- Non powered, needs external pullup resistors: [Jaycar SR1230](https://www.jaycar.com.au/rotary-encoder-with-pushbutton/p/SR1230 "Jaycar SR1230")
- Non powered, needs external pullup resistors: [Sparkfun 9117](https://www.sparkfun.com/products/9117 "Sparkfun 9117")
- Non powered, needs external pullup resistors: [Adafruit 377](https://www.adafruit.com/product/377 "Adafruit 377")
- Powered, do not fit external pullup resistors: [PiHut](https://thepihut.com/products/keyes-rotary-encoder-module "PiHut")
- Powered, do not fit external pullup resistors: [Waveshare](https://www.waveshare.com/rotation-sensor.htm "Waveshare")

Note 1: The rotary encoder I used from Jaycar produces 15 pulses per revolution. It needs two external pullup resistors (2x 47k Ohm) because the ESPHome rotary encoder platform is coded for the type of encoders that are powered (i.e you provide power and ground to them), so it does not enable the ESP internal pullup resistors. **If you buy an encoder that is powered, do NOT fit the external pullup resistors.** You may need to modify the code if the number of pulses per revolution is different to Jaycar item I used.

Note 2: Having said all that, the push button built into the encoder (activates when you push down on the shaft) does NOT need an external pullup resistor as you just configure it as a normal GPIO binary sensor in ESPHome with pullup enabled (see purple wire which goes to GPIO0).
#### Display
I2C 0.54" Quad Alphanumeric Display - [Adafruit](https://www.adafruit.com/product/1911 "Adafruit"), or [Chinese knockoff](https://www.banggood.com/4-bit-Pozidriv-0_54-Inch-14-segment-LED-Digital-Tube-Module-Red-Green-I2C-Control-2-line-Control-LED-Display-Screen-Module-p-1546304.html?rmmds=search&ID=6157216&cur_warehouse=CN "Chinese knockoff")
#### ESP32
ESP32 development board (many options) - I used the Espressif [ESP32 DevKitC V4](https://docs.espressif.com/projects/esp-idf/en/v4.1-beta1/hw-reference/get-started-devkitc.html "ESP32 DevKitC V4"). From [Amazon](https://www.amazon.com.au/Espressif-ESP32-ESP32-DEVKITC-ESP-WROOM-32-soldered/dp/B01N0SB08Q "Amazon"), [Banggood](https://www.banggood.com/ESP32-DevkitC-Core-Board-ESP32-Development-Board-ESP32-WROOM-32U32D-F-VB-VIB-S1-p-1426780.html?rmmds=buy&ID=566842&cur_warehouse=CN "Banggood"), [Mouser](https://www.mouser.com/ProductDetail/Espressif-Systems/ESP32-DevKitC?qs=sGAEpiMZZMuJ3l9lTgMBp1ZUJQFPkBG2y%2FW6zdCuiDMN%252BIbH6RhDCA%3D%3D "Mouser") and many more suppliers.

------------

## Detailed functional description:
1. Use a rotary encoder to select number of minutes on LED display. Starts at 5:00 (m:ss). Adjusts in 15s increments. Max 7:00.
2. Press button on rotary encoder to start countdown timer.
3. When countdown finishes, HA switches the iron off.
4. LED display turns off and enters idle state.
5. Adjust LED display brightness at any time by press and hold encoder button for > 0.6 seconds < 2.0 seconds, then select beteween 0.1 and 1.0
6. To finish countdown early, double click encoder button. Switch turns off immeditely.
7. To add or remove time during countdown, simply twist the encoder in steps of 15s.
8. When device is idle, user can tap rotary encoder switch to wake up device, which resets to 5:00 minutes.
9. Display turns off (enters idle mode) if user does not perform an action wihtin 30 seconds.

## Installation
1. Copy the config file `iron timer Mk2.yaml` into `/hassio/config/esphome/`
2. Copy the include file `iron timer Mk2.h` into  `/hassio/config/esphome/custom_components`

As at time of writing (April-2020), the HT16K33 4 character 14-segment alphanumeric LED display is a custom component (i.e. not relased into ESPHome), so it must be cloned into the custom components folder. To get a copy of the custom component, go to:

[HT16K33 custom component by Samuel Sieb](https://github.com/ssieb/custom_components/tree/master/lcd_ht16k33 "HT16K33 custom component by Samuel Sieb")

3. Copy or clone the whole `lcd_ht16k33` folder into a subfolder under `.../custom_components` as mentioned in step 2 above.

Your ESPHome folder structure should look like this

![folders](/Iron_timer_custom_components.png "ESPHome folder structure")

Once you've built your ESP32 and other hardware, go to your ESPHome dashboard and compile and upload the project!
