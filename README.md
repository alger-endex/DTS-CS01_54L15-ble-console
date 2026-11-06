# DTS-CS01 BLE Console

A single-page Web Bluetooth console for the DTS-CS01 e-bike derailleur controller (nRF54L15). It runs entirely in the browser: no install, no server-side code.

**Open it:** https://alger-endex.github.io/DTS-CS01_54L15-ble-console/

## What it does

**Control tab**
- Drive the motor forward or reverse at a chosen duty, stop (coast) or brake.
- Shift to gears G1–G16 (closed-loop positioning on the board).
- Show live status: direction, duty, fault, angle, gear, supply voltage, motor current. Auto status polls once a second.
- Edit the 16 gear angle setpoints. The board validates each change and keeps the table in flash.
- Teach the mechanical zero.
- Send raw text commands and see the traffic log.

**Firmware update tab**
- Read the board's MCUboot slots (version, hash, flags).
- Upload a signed image (`zephyr.signed.bin` or `dfu_application.zip`), test-boot it, reconnect and wait for the board to confirm it.
- Switch back to the image in slot 1 without uploading.
- [firmware-guide.html](firmware-guide.html) is the operator guide for this tab, in Traditional Chinese.

## Requirements

- Chrome or Edge on Windows, macOS, Linux, ChromeOS or Android. Safari and iOS do not support Web Bluetooth.
- The page must be served over `https://` (GitHub Pages does this) or `http://localhost`. Opened as a `file://` page, Web Bluetooth is unavailable.
- A board running DTS-CS01 firmware. It advertises as `DTS-CS01_xxxxxx`, where `xxxxxx` is the last 6 hex digits of its BLE address. Gear angle editing needs firmware 3.4.3 or later; firmware update needs a build with MCUboot.

## Use

1. Power the board and open the page.
2. Press **Connect** and pick `DTS-CS01_xxxxxx` from the browser's device list.
3. The board accepts one connection at a time: disconnect any phone app first.

## Commands

The Control tab sends these as text lines over the Nordic UART Service. You can also type them into the raw command box.

| Command | Action |
|---|---|
| `F`0–100 / `R`0–100 | Forward / reverse at that duty % |
| `S` / `B` | Stop (coast) / brake |
| `G`1–16 | Move to that gear's angle |
| `Z` | Teach the current position as zero |
| `A?` | Report the gear angle table |
| `A`n`=`deg | Set gear n's angle (0–300°, at least 5° above the gear below) |
| `AR` | Restore the built-in angle table |
| `?` | Report status |

## Safety

The firmware does not require BLE pairing. Anyone within Bluetooth range who opens this page can move the motor, change the stored gear angles and zero, reset the board or upload firmware. Image signatures stop foreign firmware from booting, but they do not stop the other actions. Do not use this on a bike that is being ridden.

## License

MIT, see [LICENSE](LICENSE).
