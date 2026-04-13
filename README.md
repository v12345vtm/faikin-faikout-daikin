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
   - use the official integration Daikin for the faikin-faikout
   - or esphome alternative = https://github.com/joshbenner/esphome-daikin-s21 where the mosfets will not be needed
 
7. **S10 portt**:
   - set rx tc in the esp32 webinterface
   - the original board uses fets to drive the S10 port ( maybe its inverted ?)
   - The physical interface is basic serial data, low is 0, high is 1. However the Daikin appears to operate with a 5V internal pull up on Tx and Rx lines, and a pull down. The Faikout uses a FET each way, which means internally the lines are inverted.
   - so use
   - Use a simple MOSFET level shifter:
   - BSS138 (very common)👉 This matches what RevK boards do
   - 2x pull-up resistors (e.g. 10k)






The S403 port is a non-isolated S21 port, i.e. it is at MAINS POWER levels, and dangerous. Any connected device, such as a Faikout, needs to be inside the case.

The S21 port pin out is 1:1 the same as the Faikout pin out :-
   
## 3. Troubleshooting

- **Wrong COM port?** Device Manager → Ports (COM & LPT)
- **Drivers missing?** Install CP210x/CH340 drivers
- **Connection fails?** 
  - Run CMD as Administrator
  - Add `--baud 115200` to command
  - Try different USB cable/port
