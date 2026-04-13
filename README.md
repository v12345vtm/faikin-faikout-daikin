# faikin-faikout-daikin
ESP32 dev board S1 mode

## 1. Install esptool.py (Windows 10)

**Prerequisites:** Python 3.7+ with pip installed

1. **Open Command Prompt as Administrator**
2. **Install:**
   ```bash
   pip install esptool
   ```
   *Or if needed:*
   ```bash
   python -m pip install esptool
   ```
3. **Test installation:**
   ```bash
   esptool version
   ```
   *Expected: `esptool.py v4.7` (or similar)*

**If "command not found":**
- Add Python Scripts folder to PATH:
  - `C:\Python39\Scripts` or `%APPDATA%\Python\Python39\Scripts`
  - System Properties → Environment Variables → Path → Edit → New → Add path → OK
  - Restart Command Prompt

## 2. Flash Faikout S1 Firmware

1. **Download release binaries** to a folder (all 4 `.bin` files):
   - `Faikout-S1-bootloader.bin`
   - `Faikout-S1-partition-table.bin`
   - `Faikout-S1-ota_data_initial.bin`
   - `Faikout-S1.bin`

2. **Navigate to that folder**:
   - Hold `Shift` + right-click in folder
   - "Open PowerShell window here" (or "Open command window here")
   - Type `cmd` and Enter

3. **Connect ESP32 dev board** to USB (check COM port in Device Manager)

4. **Put board in flash mode**:
   - Hold BOOT button
   - Press and release RESET button  
   - Release BOOT button

5. **Flash firmware** (replace COM3 with your port):
   ```bash
   python -m esptool -p COM3 write_flash ^
   0x1000 Faikout-S1-bootloader.bin ^
   0x8000 Faikout-S1-partition-table.bin ^
   0xD000 Faikout-S1-ota_data_initial.bin ^
   0x10000 Faikout-S1.bin
   ```


6. **Home Assistant**:
   - use the official integration Daikin
 
7. **S10 portt**:
   - set rx tc in the esp32 webinterface
   - the original board uses fets to drive the S10 port ( maybe its inverted ?)  
   
## 3. Troubleshooting

- **Wrong COM port?** Device Manager → Ports (COM & LPT)
- **Drivers missing?** Install CP210x/CH340 drivers
- **Connection fails?** 
  - Run CMD as Administrator
  - Add `--baud 115200` to command
  - Try different USB cable/port
