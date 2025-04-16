This fork adds support for LDM-BB-K1946VK035 minimal evaluation board by LDM-Systems. See branch "ldm-systems".

"uKEY" (the button at the tip of the board) is used to enter the bootloader after a reset. Since DTR line from the USB-Serial chip is not connected to nRST line, the reset has to be manual, therefore one has to press both RST and uKEY buttons, then let go of the RST button, and then let go of the uKEY button (similar to so-called "button play" in ESP32 world). Afterwards the bootloader will be waiting for 5 seconds for the flasher to connect (default bootloader timeout was increased to 5s).

## Bootloader for k1921vk035
Bootloader for K1921VK035. Is used with flasher tool [k1921vkx_flasher](https://github.com/DCVostok/k1921vkx_flasher). Bootloader firmware is loaded to flash NVR region with size 3 kB. 

### Enter to bootloader.
1. Pulldown `BOOTEN_PIN`
2. Reset MCU
3. Send byte `0x7F` to `UART0` before timeout occurs (by default is `500ms`)
4. Wait answer `PACKET_DEVICE_SIGN` (by default is `0x7EA3`)
### Support cmds
* CMD_GET_INFO
* CMD_GET_CFGWORD
* CMD_SET_CFGWORD
* CMD_WRITE_PAGE
* CMD_READ_PAGE
* CMD_ERASE_FULL
* CMD_ERASE_PAGE
* CMD_EXIT
## Upload bootloder

1. Set pin SERVEN to 3.3v
2. Hard Reset 
3. Do full service erase
```
pio run -t service_full_erase
```
1. Unset pin SERVEN
2. Hard Reset
3. Enable bootloader
```
pio run -t enable_boot
```
1. Hard Reset
2. Upload bootloader firmware to NVR region
```
pio run -t upload
```

Note: If BMOEDIS bit is reset, uploading the main firmware via jtag/swd damages the bootloader firmware.

# Testing
Test use flasher tool [k1921vkx_flasher](https://github.com/DCVostok/k1921vkx_flasher).

## Run test 
Tests runs on Vostok UNO-vn035
 ```
 cd test/test_firmware
 pio run -t run_tests --upload-port COM6
 ```
