## TEST FOR AUTOBAUD OF UART

Upload the binary file to your ESP8266 and connect to it
with a serial terminal emulator and set the speed to one of
300,1200,2400,4800,9600,19200,38400,57600,115200 baud.

Then press <enter> twice and see if the firmware detects the baudrate
correctly.

Click here https://github.com/SmallRoomLabs/esp_autobaud/raw/master/autobaud-0x00000.bin to download the image, then just flash it a location 0x00000

esptool.py write_flash 0 autobaud-0x00000.bin -fm dout
  
Please test all of the speeds and then tell me how it worked by either dropping me an email at mats.engstrom@gmail.com or open an issue here.

This is how the function looks like:
```
uint32_t AutoBaud() {
  uint8_t v;
  for (;;) {
    uart_div_modify(UART0, (PERIPH_FREQ * 1000000) / 115200);
    v=GetRxChar();  // Wait for one character received
    if (v==0x0D) return 115200;
    if (v==0xE6) return 57600;
    if (v==0x1C) return 38400;
    if (v==0xE0) return 19200;
    ets_delay_us(50000); // Delay to trap possible additional chars
    while (GetRxCnt()) GetRxChar(); // Flush receive buffer
    uart_div_modify(UART0, (PERIPH_FREQ * 1000000) / 9600);
    v=GetRxChar();
    if (v==0x0D) return 9600;
    if (v==0xE6) return 4800;
    if (v==0x78) return 2400;
    if (v==0x80) return 1200;
    if (v==0x00) return 300;
    ets_delay_us(50000); // Delay to trap possible additional chars
    while (GetRxCnt()) GetRxChar();  // Flush receive buffer
  }
  return 0;
}
```
