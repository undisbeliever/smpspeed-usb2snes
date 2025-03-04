# smpspeed-usb2snes

A utility to written by undisbeliever to monitor and log S-SMP clock speeds and related metrics from SNES/Super Famicom hardware using an FXPAK PRO/SD2SNES via USB2SNES when running the smpspeed ROM written by lidnariq.

## Description

The smpspeed-usb2snes script connects to a SNES/Super Famicom via USB2SNES (using an SD2SNES or FXPAK PRO) and records the S-SMP clock speed and related timing information shown on screen. The tool collects data at specified intervals and logs it to a CSV file for analysis.

This utility is designed for SNES hardware enthusiasts, developers, and researchers who want to measure and analyze the timing characteristics of the SNES audio processor.

## Requirements

- Python 3.6 or higher
- USB2SNES compatible device (SD2SNES, FXPAK PRO, etc.)
- QUsb2SNES or compatible USB2SNES server running
- SNES hardware running smpspeed.sfc measurement ROM

### Dependencies

- websocket-client==1.8.0

## Installation

1. Clone this repository or download the source code
2. Install the required dependencies:

```bash
pip install --user -r requirements.txt
```

## Usage

```bash
python smpspeed.py [options]
```

### Options

- `-a, --address` - WebSocket address of the USB2SNES server (default: `ws://localhost:8080`)
- `-i, --interval` - Interval between reads in seconds (default: 5)
- `-o, --csv-output` - Custom output CSV filename (default: `smpspeed-YYYYMMDDHHMMSS.csv`)

### Examples

Basic usage with default settings:
```bash
python smpspeed.py
```

Specify a custom WebSocket address and shorter polling interval:
```bash
python smpspeed.py --address ws://192.168.1.100:8080 --interval 2
```

Specify a custom output filename:
```bash
python smpspeed.py --csv-output my-snes-measurements.csv
```

## Output Format

The tool generates a CSV file with the following columns:

- Time - Timestamp of the measurement
- SNES PPU (Hz) - PPU frequency 
- Meaning (μs) - Timing measurement in microseconds
- Slowest (μs) - Slowest timing measurement
- Fastest (μs) - Fastest timing measurement
- S-SMP clock (Hz) - S-SMP clock frequency
- S-SMP relative (ppm) - S-SMP clock relative to expected (parts per million)
- Slowest clock (Hz) - Slowest measured S-SMP clock
- Fastest clock (Hz) - Fastest measured S-SMP clock
- DSP sample rate (Hz) - DSP sample rate

## How It Works

The tool connects to a USB2SNES device running the smpspeed.sfc ROM. It reads data from SNES VRAM that contains timing information displayed by the ROM, verifies the data by taking multiple reads, and logs the results to a CSV file.

The tool includes error handling for connection issues and will attempt to reconnect or wait for valid data if there are reading issues.

## License

This project is licensed under the MIT License - see the copyright notice in the source code for details.

## Acknowledgments

- Uses the Usb2Snes class from the unnamed-snes-game Resources project by undisbeliever who wrote the initial gist that this repo was based on
