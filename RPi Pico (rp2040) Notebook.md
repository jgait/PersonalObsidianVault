#Embedded #RP2040 
## Link Index
All sorts of interesting things from around the Web

| Thing-a-ma-jig                                                                              | Description                                            |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| [Alex Wilsons Blog](https://www.alexdwilson.dev/how-to-program-raspberry-pi-pico-with-rust) | Some guys explorations of using the RPi Pico with Rust |
| [Cornell ECE4760](https://ece4760.github.io/)                                               | A great embedded system design course using RPi Pico   |
| [EarlPhilhower Core](https://arduino-pico.readthedocs.io/en/latest/)                        | A great feature rich arduino core for Pico             |

## Platform IO and EarlPhilhower Core
[Setting up on Platformio](https://arduino-pico.readthedocs.io/en/latest/platformio.html)
[General Earlcore docs](https://arduino-pico.readthedocs.io/en/latest/index.html)

**Getting Drivers Working:** Had to use [Zadig](https://zadig.akeo.ie/) to install the correct drivers for the pico
> *A.* Hold `BOOTSEL1` and plug in Pico then set `RP2 Boot (Interface 1)` to `WinUSB`
> *B.* Unplug and re-plug and set `Pico` driver to `USB Serial`

**Note:** On first upload or if your sketch crashes you will need to drag the .uf2 file found in the .pio folder to the pico. Hold bootsel and unplug then replug the pico to get it to pop up as a storage you can drag the .uf2 into.

### Targeting a specific board within the earlphilhower core
To target a specific board we need to modify the `platform.ini` file 
1. To find the board we want look in the `tools/json/` directory in the [Arduino-Pico Github](https://github.com/earlephilhower/arduino-pico/tree/master) and find the board we want. Note the file name of the `.json` file that details the target board 
2. In the `platform.ini` file set the `board` parameter equal to the file name of the target board description (without the `.json`) part