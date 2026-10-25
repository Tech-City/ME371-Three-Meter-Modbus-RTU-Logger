# ME371 Three-Meter Modbus RTU Logger

Python logger for polling three ME371 meters over an RS485 bus and saving timestamped readings to Excel.

## Overview

The application was tested with meter IDs `1`, `2` and `3` using a USB-RS485 adapter connected to a Windows computer. It reads configured Modbus register groups and appends each meter's results to `ME371_Meter_Report.xlsx`.

The serial settings used during testing were 9600 baud, 8 data bits, no parity and 1 stop bit. The logger waits 10 seconds between meters and targets a 60-second interval between the start of polling cycles.

## Hardware setup

![Three ME371 meters](hardware/three_meter_physical_setup.jpg)

Photograph of the test setup. The picture documents the prototype and is not a wiring schematic or an approved installation method.

## Example output

![Excel readings](screenshots/excel_readings.png)

The screenshot shows timestamped records for all three meter IDs. Voltage readings are around 229–230 V on the displayed channels. Current and power readings shown are zero; the cause has not yet been established.

## Requirements

- Windows computer with Python 3 installed
- Three ME371 meters configured with unique Modbus IDs
- USB-RS485 adapter and correctly wired RS485 bus
- `pymodbus`, `pyserial` and `openpyxl`

Install the Python dependencies from the repository directory:

```powershell
py -m pip install -r requirements.txt
```

## Configure and run

1. Open `src/me371_poll_excel.py`.
2. Set `PORT` to the adapter's serial port (the test setup used `COM9`).
3. Check `BAUDRATE` and `METER_IDS` against the meter configuration.
4. Close `ME371_Meter_Report.xlsx` in Excel if it is already open.
5. Run:

   ```powershell
   py src/me371_poll_excel.py
   ```

The workbook is created beside the script in `src/`. Stop polling with `Ctrl+C`.

## Register notes and limitations

The voltage register pair was checked using big-endian IEEE-754 32-bit float decoding. The script currently uses the same decoding approach for other register groups, but their data types, units and scaling have not all been verified against the ME371 register documentation. The energy values in particular need further investigation. Treat the results as bench-test data, not validated metering or billing data.

See [`docs/register_map.md`](docs/register_map.md) and [`docs/test_observations.md`](docs/test_observations.md) for the current notes.

## Safety

Mains voltage can cause fatal injury. Do not adjust wiring while equipment is energized. Use appropriately rated protection, insulated terminals and enclosures, and have the installation checked by a competent person. This repository does not provide a verified terminal-by-terminal wiring guide.

## Files

- `src/me371_poll_excel.py` — polling and Excel logging application
- `hardware/three_meter_physical_setup.jpg` — physical setup photograph
- `screenshots/excel_readings.png` — sample output screenshot
- `docs/register_map.md` — register address notes
- `docs/test_observations.md` — observations from testing
- `requirements.txt` — Python dependencies

## Author

Adubina Victor

## License

No license has been added. All rights are reserved by the author unless permission is granted.
