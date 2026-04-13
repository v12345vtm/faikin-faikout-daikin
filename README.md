# faikin-faikout-daikin
esp32 dev board s1 mode


## Flash Faikout S1 Firmware (ESP32)

```bash
python -m esptool -p COM3 write_flash ^
0x1000 Faikout-S1-bootloader.bin ^
0x8000 Faikout-S1-partition-table.bin ^
0xD000 Faikout-S1-ota_data_initial.bin ^
0x10000 Faikout-S1.bin
