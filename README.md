## TEST FOR AUTOBAUD OF UART

Upload the binary file to your ESP8266 and connect to it
with a serial terminal emulator and set the speed to one of
300,1200,2400,4800,9600,19200,38400,57600,115200 baud.

Then press <enter> twice and see if the firmware detects the baudrate
correctly.

Click here https://github.com/SmallRoomLabs/esp_autobaud/raw/master/autobaud-0x00000.bin to download the image, then just flash it a location 0x00000

esptool.py write_flash 0 autobaud-0x00000.bin -fm dout
  
Please test all of the speeds and then tell me how it worked by either dropping me an email at mats.engstrom@gmail.com or open an issue here.

