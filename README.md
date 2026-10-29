ESP32 Apophis 2029 Flyby Countdown Tracker
A live countdown clock for asteroid Apophis's close approach on April 13, 2029, built with an ESP32 and a 128×64 yellow/blue OLED display.

Hardware
ESP32 development board

0.96" OLED, 128×64, I2C (yellow/blue two-color, SH1106 or SSD1306)

4 jumper wires

USB cable

Wiring (I2C)
OLED Pin	ESP32 Pin
VCC	3.3V
GND	GND
SDA	GPIO 21
SCL	GPIO 22
Check your module's silkscreen — some boards swap VCC/GND. Keep wires under 20 cm.

Library
Install via Arduino IDE → Sketch → Include Library → Manage Libraries:

U8g2 by oliver

Restart the IDE after installing.

Configuration
Edit these lines at the top of the sketch:

cpp
const char* ssid     = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
Driver Selection (Important)
Yellow/blue OLEDs are commonly SH1106, not SSD1306. Using the wrong driver causes overlapping text.

Default line in the sketch:

cpp
U8G2_SH1106_128X64_NONAME_F_HW_I2C u8g2(U8G2_R0, U8X8_PIN_NONE);
If text still overlaps, comment that line and uncomment:

cpp
U8G2_SSD1306_128X64_NONAME_F_HW_I2C u8g2(U8G2_R0, U8X8_PIN_NONE);
Reflash. One of the two will render clean.

Data Used
Value	Source
Closest approach time	2029-04-13 21:46 UTC (JPL nominal)
Distance	~38,000 km center-to-center (~32,000 km from surface)
Relative velocity	~7.4 km/s
Behavior
Connects to Wi-Fi (20 s timeout, continues without it)

Syncs time via NTP (UTC)

Rejects invalid time (pre-2020) before computing

Displays countdown: days d HH:MM:SS

Updates every second

Troubleshooting
Symptom	Fix
Blank screen	Try OLED address 0x3D instead of 0x3C
Overlapping text	Swap SH1106 ↔ SSD1306 line (see above)
"Waiting for NTP..." forever	Check Wi-Fi credentials / signal
Garbage pixels	Lower I2C clock to 100 kHz (already default)
Works intermittently	Add 100 µF cap across OLED VCC/GND
Notes
Color split (yellow top, blue bottom) is fixed in hardware — layout is designed around it

Title renders in yellow zone (y ≤ 15), data in blue zone (y ≥ 16)

