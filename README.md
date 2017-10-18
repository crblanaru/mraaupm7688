This is my first attempt at recreating the makefiles for the latest version of libupm (1.5.0) and libmraa (1.8.0) on the Linkit Smart 7688.
Please consider this an untested, development version if you try to build the packages yourself at this stage. Also please note that as I'm including all the sensors, you really need to have the SD card set up for the Linkit as the size of libupm is > 10M. Try to take out the sensors you are not interested in to save space.

This project started from the need to use newer sensors that were not present in libupm when they were built for the 7688.

Tested so far (and works):
I2C: bmp280, ssd1306
