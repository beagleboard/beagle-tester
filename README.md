# beagle-tester

## Buildroot images

Download the latest releases bundled in Buildroot at https://github.com/beagleboard/buildroot/releases

## Supported boards

* [BeagleBone Black](#beaglebone-black)
* [BeagleBone Black Industrial](#beaglebone-black)
* [BeagleBone Black Wireless](#beaglebone-black-wireless)
* [BeagleBone Blue](#beaglebone-blue)
* [BeagleBoard-xM](#beagleboard-xm)
* [PocketBeagle GamePup Cape](#gamepup)
* [PocketBeagle TechLab Cape](#techlab)

## Supported scanners

* [Datalogic QuickScan L](http://www.datalogic.com/eng/products/automatic-data-capture/general-duty-handheld-scanners/quickscan-l-qd2300-pd-166.html) - VID:05f9 "PSC Scanning, Inc." PID:2204 
* [Intermec SG20 General Duty 1D/2D Handheld Scanner](http://www.intermec.com/products/scansg20t/) - VID:067E PID: 0801

## _DUT_ software setup

For production, the boards should be flashed with an approved production image ahead of beginning this test. No additional software setup steps should be performed. For non-capes, the first 12 characters of the EEPROM should also be valid ahead of running this test, though the additional serial number characters need not and should not be programmed until this test is run.

If not already setup, on a recent [BeagleBoard.org Debian image](https://beagleboard.org/latest-images), perform:

    apt-get update
    DEBIAN_FRONTEND=noninteractive apt-get install -y roboticscape
    cd /opt/source
    git clone https://github.com/beagleboard/beagle-tester
    cd beagle-tester
    make && make install

## _Host_ software setup

If you are running Windows as the _host_, make sure you [disable the Windows Firewall for ICMPv4 packets](https://kb.iu.edu/d/aopy).

## Serial number barcode format

Each board that has an on-board EEPROM should have an associated 16 digit serial number placed onto a barcode on the board.

### The first 4 ASCII characters indicate the board type:

* BeagleBone Black - 00C0
* BeagleBone Black Industrial - EIA0
* BeagleBone Black Wireless - BWA5
* BeagleBone Blue - BLA2
* PocketBeagle GamePup Cape - PC00
* PocketBeagle TechLab Cape - PC01

### The second 4 characters should: 
* For non-capes, indicate the manufacturing week in the format YYWW, where YY is currently 19 and WW is currently 03.
* For capes, indicate the revision level, such as 00A3.

### The next 4 characters should:
* For capes, indicate the manufacturing week in the format YYWW, where YY is currently 19 and WW is currently 03. Capes have exclusive manufacturers, so the manufaturer-specific product code can be left off.
* For non-capes, be a manufacturer-specific product code. If you are a new manufacturer, please choose something unique you can use to identify your boards.

Manuracturer-specific allocations include, but are not limited to: 

* BBGW for GHI manufactured BeagleBone Black Wireless
* ELnn for Embest manufactured BeagleBone Blue
* SBB for Seeed manufactured BeagleBone Black
* SBI for Seeed manufactured BeagleBone Black Industrial
* GPB for GHI manufactured PocketBeagle
* SPB for Seeed manufactured PocketBeagle

### The final 4 characters should:
* Be a sequential decimal number. If more than 10,000 boards are manufactured that week, roll over the top digit to an ASCII hex character.

# BeagleBone Black

## Required equipment

* _TV_: HDMI TV capable of 1280x720p60 (720p) (HDMI monitor if no audio testing) with HDMI-to-microHDMI cable
* _Router_: Ethernet router (configured to answer DHCP requests and answer pings on the provided gateway) with Ethernet cable
* _Host_: BeagleBone Black or other computer (configured to make DHCP requests over USB RNDIS interface and answer pings)
* _Scanner_: A supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Power_: Approved 5V power brick
* _DUT_: BeagleBone Black (device) under test

## Test steps

1. Connect microHDMI on _DUT_ to _TV_
2. Connect Ethernet on _DUT_ to _router_
3. Connect a wire from TP4 to ground to enable EEPROM writing of board revision and serial number
4. Connect _DUT_ to _power_
5. Wait for the BeagleBoard.org desktop to show (should be under 2 minutes)
6. Connect _scanner_
7. Wait for the CISPR test animation and audio playback (should be under 15 seconds) ![CISPR image][cispr]
8. Connect USB client port on _DUT_ to _host_
9. Scan the 16 character barcode
10. Pass or fail will be indicated by a respectively green or red box on the _TV_
11. Disconnect _scanner_
12. Disconnect _host_
13. Disconnect _power_
14. Disconnect remaining devices

# BeagleBone Black Wireless

## Required equipment

* _TV_: HDMI TV capable of 1280x720p60 (720p) (HDMI monitor if no audio testing) with HDMI to microHDMI cable
* _AP_: BeagleBone Black Wireless acting as a WiFi access point (should be default on production image) (SSID: BeagleBone-XXXX, PSK: BeagleBone)
* _Host_: BeagleBone Black Wireless or other computer (configured to make DHCP requests over USB RNDIS interface and answer pings)
* _Scanner_: Supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Power_: Approved 5V power brick
* _DUT_: BeagleBone Black Wireless (device) under test

## Test steps

1. Ensure _AP_ is functioning nearby 
2. Connect microHDMI on _DUT_ to _TV_
3. Connect a wire from TP1 to ground to enable EEPROM writing of board revision and serial number
4. Connect _DUT_ to _power_
5. Wait for the BeagleBoard.org desktop to show (should be under 2 minutes)
6. Connect USB host on _DUT_ to _scanner_
7. Wait for the CISPR test animation and audio playback (should be under 15 seconds) ![CISPR image][cispr]
8. Connect USB client on _DUT_ to _host_
9. Scan the 16 character barcode
10. Pass or fail will be indicated by a respectively green or red box on the _TV_
11. Disconnect _scanner_
12. Disconnect _host_
13. Disconnect _power_
14. Disconnect remaining devices

# BeagleBone Blue

## Required equipment

* _AP_: BeagleBone Blue acting as a WiFi access point (use beagle-tester-host-setup.sh script) (SSID: BeagleBone-XXXX, PSK: BeagleBone)
* _Host_: BeagleBone Blue or other computer (configured to make DHCP requests over USB RNDIS interface and answer pings, use beagle-tester-host-setup.sh script)
* _Scanner_: Supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Power_: Approved 12V power brick
* _DUT_: BeagleBone Blue (device) under test

## Test steps

1. Ensure _AP_ is functioning nearby
2. Connect a wire from WP to GND to enable EEPROM writing of board revision and serial number
3. Connect _DUT_ to _power_
4. Connect USB host on _DUT_ to _scanner_
5. Wait for the G and R LEDs to be lit
6. Connect USB client on _DUT_ to _host_
7. Scan the 16 character barcode
8. Pass or fail will be indicated by respectively G or R led flashing exclusively (blue LEDs 0-3 will flash indicating the test number executing and if blue LEDs 0-3 stop flashing before G or R begin flashing, then the board hung and failed)
9. Disconnect _scanner_
10. Disconnect _host_
11. Disconnect _power_

# BeagleBoard-xM

## Required equipment

* _TV_: DVI-D/HDMI TV capable of 1280x1024 with HDMI cable
* _Router_: Ethernet router (configured to answer DHCP requests and answer pings on the provided gateway)
* _Host_: BeagleBone Black or other computer (configured to make DHCP requests over USB RNDIS interface and answer pings)
* _Scanner_: Supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Flashdrives_: Three (3) USB 2.0 HS capable flash drives
* _Speaker_: Speaker with 1/8" audio patch cable
* _Power_: Approved 5V power brick
* _DUT_: BeagleBoard-xM (device) under test

## Test steps

1. Connect HDMI on _DUT_ to _TV_
2. Connect Ethernet on _DUT_ to _router_
3. Connect 3x USB host on _DUT_ to _flashdrives_
4. Connect audio output on _DUT_ to _speaker_
5. Connect _DUT_ to _power_
6. Wait for the BeagleBoard.org desktop to show (should be under 2 minutes)
7. Connect USB host on _DUT_ to _scanner_
8. Wait for the CISPR test animation and audio playback (should be under 15 seconds) ![CISPR image][cispr]
9. Connect USB client on _DUT_ to _host_
10. Scan a barcode to begin the test
11. Pass or fail will be indicated by a respectively green or red box on the _TV_ ![xM pass image][xm-pass]
12. Disconnect _scanner_
13. Disconnect _host_
14. Disconnect _power_
15. Disconnect remaining devices

[cispr]: https://raw.githubusercontent.com/beagleboard/beagle-tester/main/images/itu-r-bt1729-colorbar-3200x1800.png
[xm-pass]: https://farm1.staticflickr.com/531/31402272653_86721d4fa5_o_d.png

# GamePup

## Required equipment

* _Host_: PocketBeagle used to execute the test with programmed microSD inserted
* _Scanner_: A supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Power_: Approved 5V power brick with microUSB cable
* _DUT_: GapePup Cape (device) under test

## Test steps

1. Connect _host_ and _DUT_
2. Connect a wire across the EEPROM jumper to enable EEPROM writing of board revision and serial number
3. Connect _power_ to _host_
4. Wait for the LCD on _DUT_ to turn on (should be under 30 seconds)
5. Connect _scanner_ to _DUT_
6. Wait for the CISPR test animation and audio playback (should be under 5 seconds) ![CISPR image][cispr]
7. Scan barcode to begin the test
8. Observe tone played from _DUT_
9. Pass or fail will be indicated by a respectively green or red box on the LCD on _DUT_
10. Observe 2 red LEDs on _DUT_ lit steadily
11. Disconnect _scanner_
12. Press buttons to observe key presses sent to the console and different tone played per key
13. Disconnect _power_
14. Disconnect remaining devices

## Buttons

* Select: 5
* Start: 1
* Left-Up: up arrow, ^\[\[A
* Left-Down: down arrow, ^\[\[B
* Left-Right: right arrow, ^\[\[C
* Left-Left: left arrow, ^\[\[D
* Right-Up: ESC
* Right-Right: TAB
* Right-Left: p
* Right-Down: ENTER

# TechLab

## Required equipment

* _Host_: PocketBeagle used to execute the test with programmed microSD inserted
* _Scanner_: A supported barcode scanner (listed above) (along with a suitable 16 character barcode on the device under test)
* _Power_: Approved 5V power brick with microUSB cable
* _DUT_: TechLab Cape (device) under test

## Test steps

1. Connect _host_ and _DUT_
2. Connect a wire across the EEPROM jumper to enable EEPROM writing of board revision and serial number
3. Connect _power_ to _host_
4. Wait for "heartbeat" on _host_ LED USR0
5. Connect _scanner_ to _DUT_
6. Wait for all _host_ USRx LEDs to be on solid
7. Scan barcode to begin the test
8. Observe all 14 seven segment LEDs to turn on
9. Observe tone played from _DUT_
10. Observe RGB LED cycle through red-green-blue
11. Observe all _host_ USRx LEDs to be on solid again
12. Observe RGB LED to be flashing green (not red)
13. Press the L button to observe the left seven segment display to turn off
14. Press the R button to observe the right seven segment display to turn off
15. Disconnect _scanner_
16. Disconnect _power_
17. Disconnect remaining devices
