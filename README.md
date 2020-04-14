# Iron-Timer-Mk2
## ESPHome based Iron Timer
Never forget to turn your clothes iron off again! Plug your clothes iron into a WiFi power switch which you've previously integrated into Home Assistant (HA). A rotary encoder allows you to input countdown time in minutes and seconds. Press the encoder button down, and the timer starts counting down. When it reaches zero, it instructs HA to switch off the iron. A 14-segment alphanumeric LED display shows the selected and remaining time.

![cct_diagram](/Circuit_diagram_Iron_Timer.png "Iron timer circuit diagram")

## Detailed description:
1. Use a rotary encoder to select number of minutes on LED display. Starts at 5:00 (m:ss). Adjusts in 15s increments. Max 7:00.
2. Press button on rotary encoder to start countdown timer.
3. When countdown finishes, HA switches the iron off.
4. LED display turns off and enters idle state.
5. Adjust LED display brightness at any time by press and hold encoder button for < 0.6seconds, then select beteween 0.1 and 1.0
6. To finish countdown early, double click encoder button. Switch turns off immeditely.
7. To add or remove time during countdown, simply twist the encoder in steps of 15s.
8. When device is idle, user can tap rotary encoder switch to wake up device, which resets to 5:00 minutes.
9. Display turns off (enters idle mode) if user does not perform an action wihtin 30 seconds.

## Installation
1. Copy the config file `iron timer Mk2.yaml` into `/hassio/config/esphome/`
2. Copy the include file `iron timer Mk2.h` into  `/hassio/config/esphome/custom_components`

As at time of writing (April-2020), the HT16K33 4 character 14-segment alphanumeric LED display is a custom component (i.e. not relased into ESPHome), so it must be cloned into the custom components folder. To get a copy of the custom component, go to:

[HT16K33 custom component by Samuel Sieb](https://github.com/ssieb/custom_components/tree/master/lcd_ht16k33 "HT16K33 custom component by Samuel Sieb")

3. Copy or clone the whole `lcd_ht16k33` folder into a subfolder under `.../custom_components` as mentioned in step 11 above.

Your ESPHome folder structure should look like this

![folders](/Iron_timer_custom_components.png "ESPHome folder structure")

Once you've built your ESP32 and other hardware, go to your ESPHome dashboard and compile and upload the project!
