# Anti-AFK Tool

A Python-based automation utility designed to prevent users from being marked as "Away From Keyboard" (AFK) in games, messaging apps, or other software. It provides a customizable Graphical User Interface (GUI) built with Tkinter, alongside an alternative Command-Line Interface (CLI) script that features image detection.

## Features

- **Graphical User Interface (GUI)**: An intuitive, easy-to-use interface to configure and toggle various anti-AFK mechanisms without writing code.
- **Custom Key Presses**: 
  - Simulate pressing or holding random keys.
  - Set up specific secondary and tertiary keys with configurable execution delays.
- **Mouse Movements**: Simulate mouse movements across the screen (Left/Right or Up/Down routines) to mimic human activity.
- **Automated Clicking**:
  - **Random Clicks**: Clicks the mouse at random intervals.
  - **Auto Left Click**: Clicks continuously at a fixed interval.
  - **Batch Multi-Click**: Performs a set number of clicks in quick succession periodically.
- **Image Recognition (CLI script)**: Automatically detects an on-screen prompt (e.g., an "I'm still here" button) via image matching and clicks it.
- **Multithreaded**: Ensures the GUI remains completely responsive while the automation loops run in the background.

## Prerequisites

- **Python 3.x**
- Install the required external dependencies using `pip`:

```bash
pip install pyautogui keyboard pynput
```

*(Note: On Windows, `tkinter` is included with standard Python installations. If you are on Linux, you may need to install it via your package manager, e.g., `sudo apt-get install python3-tk`)*

## Usage

### 1. GUI Version (`main.py`)
This is the primary script offering a visual configuration window.

1. Run the script:
   ```bash
   python main.py
   ```
2. Check the boxes for the actions you wish to enable (e.g., *Press Random Keys*, *Move Mouse Left/Right*, *Enable Auto Left Click*).
3. Adjust the corresponding delays and intervals in the text fields.
4. Click **Start** to begin the anti-AFK routine.
5. Click **Stop** to halt all automated actions.

### 2. CLI Hotkey Version (`anti_afk.py`)
A lightweight, keyboard-shortcut-driven alternative that also includes image detection for AFK prompts.

1. Ensure you have an image of the button you want to detect saved as `imgs/start.png` in the project directory.
2. Run the script:
   ```bash
   python anti_afk.py
   ```
3. Press **`F2`** to start the anti-AFK loop. The script will periodically scan the screen for the `start.png` image and click it if it appears.
4. Press **`F3`** at any time to stop the script.

## Disclaimer

This tool is created for educational purposes and personal use. Please be aware of the Terms of Service (ToS) or End User License Agreements (EULA) of the games or applications you use this with, as using macro or automation tools may result in account bans or penalties. Use at your own risk.
