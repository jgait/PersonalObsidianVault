#Arduino #RaspberryPi #PlatformIO

# The Scenario
Robot with Raspberry Pi and Arduino Nano. We want to be able to SSH into the raspberry pi and then use the PlatformIO CLI to upload code the the attached Arduino. 
# Getting it working
1) Installed python3, python3-pip, and python3-venv 
2) Installed PlatformIO with the "Super Quick" two liner install from PlatformIO docs
3) Added `platformio.exe` path to the `~/.bashrc` file in order to enable command line discovery
4) Navigate to project folder and upload with `pio run -t upload`, discover this doesn't work and that the com port cannot be accessed
5) Research stuff
6) Check com ports with `sudo dmesg | tail -f` 
7) Discover that a braille keyboard driver is stealing the Arduino and locking up the com port. See [stack exchange](https://askubuntu.com/a/1419499)
8) Uninstall the braille driver with `sudo apt remove brltty`
9) Profit