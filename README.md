# USB PROJECT

The purpose of this project is to create a USB device with a game controller connected to an Arduino Uno using libusb-1.0 library. To test the program the pacman4console game was used from: `https://github.com/YoctoForBeaglebone/pacman4console.git`, and was adapted to the device.

## PREREQUIREMENTS :

- `ncurses` library to launch the game.
- `libusb-1.0` library to access the usb device.
- `dfu-programmer` utility to program ATMega16u2.
- `avr-libc` and `avrdude` library and compiler to program ATMega328p.
- have root right.

## MAIN PROGRAM :

### INSTALLING

To install the program type :
~~~ 
$  sudo make install
~~~
- To compile type only `make`.
- And to uninstall type `sudo make clean`.

### RUNING

To run the program type :
~~~
$  sudo ./pacman
~~~

Then direct the pacman with the buttons, exit by pressing the analog pad, and pause by pressing the analog pad and the bottom button.

The LED 13 stays on during the game.

## ARDUINO PROGRAM :

If the device is not recognized, the Arduino card must be reprogrammed.

### ATMega328p

The ATMega328p files are in `atmega328p`. The joystick is pluged in `PORTD & 0b01111100` and managed by interruption with the `PCINT2` vector.

To reprogramme the microcontroller it's necessary to flash the `ATMega328p` with the Arduino core program :

- Short-circuit the `ATMega16u2` reset and ground lines present on the `ICSP` of this microcontroller.
- The Arduino must no longer be listed as a serial port in an `lsusb`, you can then execute the following commands:
~~~
$  cd atmega328p
$  make all
$  dfu-programmer atmega16u2 erase
$  dfu-programmer atmega16u2 flash Arduino-usbserial-uno/Arduino-usbserial-uno.hex
$  dfu-programmer atmega16u2 reset
~~~
- Disconnect and reconnect your Arduino, your program must be active on `ATMega16u2`.
~~~
$  sudo make upload
$  make clean
~~~

If the device is not recognized modify the `Makefile`, particularly the port `/dev/ttyACM0`.

### ATMega16u2

The `ATMega16u2` files are in `lufa/PolytechLille/atmega16u2`. `0x0002` is the device id vendor and id product.

To reprogramme the microcontroller :

- Short-circuit the `ATMega16u2` reset and ground lines present on the `ICSP` of this microcontroller.
- The Arduino must no longer be listed as a serial port in an `lsusb`, you can then execute the following commands:
~~~
$  cd lufa/PolytechLille/atmega16u2
$  make all
$  dfu-programmer atmega16u2 erase
$  dfu-programmer atmega16u2 flash PAD.hex
$  dfu-programmer atmega16u2 reset
$  make clean
~~~
- Disconnect and reconnect your Arduino, your program must be active on `ATMega16u2`.
