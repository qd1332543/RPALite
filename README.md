# RPALite - An Open Source RPA (Robotic Process Automation) Programming Library for Python and Robot Framework

| [English](README.md) | [中文](README-zh.md) |

[![PyPI](https://img.shields.io/pypi/v/RPALite?color=blue&label=PyPI%20Package)](https://pypi.org/project/RPALite/)
[![License](https://img.shields.io/github/license/jieliu2000/RPALite)](https://github.com/jieliu2000/RPALite/blob/main/LICENSE)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/RPALite)](https://www.python.org/downloads/)

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Platform Support](#platform-support)
- [Performance Optimization](#performance-optimization)
- [Documentation](#documentation)
- [Installation](#installation)
- [Quick Start](#quick-start)
  - [Python](#python)
  - [Robot Framework](#robot-framework)
- [Contribution Guidelines](#contribution-guidelines)

## Introduction

RPALite is an open-source RPA (Robotic Process Automation) library. You can use RPALite through Python or [Robot Framework](https://robotframework.org/) to achieve various automation tasks.

RPALite now supports both Windows and macOS platforms, providing cross-platform automation capabilities.

## Features

RPALite supports the following operations:

- Launching applications
- Finding applications by name or ClassName
- Closing applications
- Mouse clicking on specific text
- Locating and inputting into text boxes based on placeholders or labels
- Mouse clicking based on coordinates
- Support for left-click, right-click, and double-click operations
- Locating controls based on control names, classes, or Automation IDs (Windows) and getting their coordinates
- Image-based location. You can pass a partial screenshot to RPALite to return the coordinates of the corresponding part on the screen.
- Screen recording capabilities
- Clipboard operations
- Advanced keyboard input support with special keys and combinations
- Window management (maximize, minimize, show desktop)

## Platform Support

### Windows
- Full automation support including UI controls
- Windows-specific features like UI Automation
- Administrative privileges may be required for some features

### macOS
- Basic automation support including mouse and keyboard control
- Window management and application control
- Requires accessibility and screen recording permissions
- Some system-wide shortcuts may not be interceptable

## Performance Optimization

The most time-consuming operations in RPALite are image recognition and OCR. For OCR, we use [EasyOCR](https://github.com/JaidedAI/EasyOCR). EasyOCR runs more efficiently on computers with dedicated GPUs and CUDA support. If you find RPALite running slowly, consider running it on a computer with a dedicated GPU and CUDA support and installing the appropriate version of PyTorch.

## Documentation

In the following sections, we provide a [Quick Start Guide](#quick-start) to give you a basic understanding of RPALite.

Here are links to more detailed documentation:

- [Programming Guide for Using RPALite in Python](docs/en/python/guide.md)
- [Programming Guide for Using RPALite in Robot Framework](docs/en/robot/guide.md)

In addition to the above documents, we provide an English version of the Robot Framework Library documentation, which you can access through the [Online Robot Framework Documentation](https://jieliu2000.github.io/RPALite/docs/en/robot/RPALite.html). If you prefer to view it locally, you can open the [Robot Framework Library documentation in the project directory](docs/en/robot/RPALite.html).

## Installation

You can install RPALite via pip:

```bash
pip install RPALite
```

Platform-specific dependencies will be automatically installed based on your operating system.

## Quick Start

As mentioned earlier, you can use RPALite with Python or Robot Framework. Here are some examples:

### Python

#### Windows Example

Below is an example of using RPALite to operate Windows Notepad:

```python
from RPALite import RPALite
rpalite = RPALite()

# Show the desktop
rpalite.show_desktop()

# Run Notepad and input some text
rpalite.run_command("notepad.exe")
rpalite.input_text("This is a demo using RPALite.\n")

# Find the Notepad app and close it
app = rpalite.find_application(".*Notepad")
rpalite.close_app(app)
```

#### macOS Example

Here's an example of using RPALite to operate Notes app on macOS:

```python
from RPALite import RPALite
rpalite = RPALite()

# Launch Notes app
rpalite.run_command("open -a Notes")

# Find and control the Notes app
app = rpalite.find_application("Notes")
if app:
    # Maximize the window
    rpalite.maximize_window(app)
    
    # Type some text
    rpalite.input_text("This is a demo using RPALite on macOS.\n")
    
    # Use keyboard shortcuts
    rpalite.send_keys("^s")  # Save (Command+S)
    
    # Close the app
    rpalite.close_app(app)
```

Note: On macOS, you'll need to grant the following permissions:
- Accessibility (for keyboard and mouse control)
- Screen Recording (for screen capture and OCR)

You can grant these permissions in System Preferences > Security & Privacy > Privacy.

### Advanced Keyboard Input Examples

```python
# Simple text input
rpalite.send_keys("Hello World")

# Special keys
rpalite.send_keys("{ENTER}")
rpalite.send_keys("{ESC}")

# Key combinations
rpalite.send_keys("^c")          # Control+C
rpalite.send_keys("%{F4}")       # Alt+F4
rpalite.send_keys("+(abc)")      # Shift+ABC (uppercase)

# macOS specific
rpalite.send_keys("#c")          # Command+C (Copy)
rpalite.send_keys("#v")          # Command+V (Paste)
rpalite.send_keys("#w")          # Command+W (Close window)
```

### Robot Framework

#### Windows Example

Below is an example of using RPALite to operate Windows Notepad:

```robotframework
*** Settings ***
Library    RPALite

*** Test Cases ***
Test Notepad
    Send Keys    {VK_LWIN down}D{VK_LWIN up}
    Run Command    notepad.exe
    ${app} =     Find Application    .*Notepad
    Maximize Window    ${app}
    Input Text    This is a demo using RPALite.
    Close App    ${app}
```

#### macOS Example

Here's an example of using RPALite with Robot Framework on macOS to operate the Notes app:

```robotframework
*** Settings ***
Library    RPALite

*** Test Cases ***
Test Notes App
    # Launch Notes application
    Run Command    open -a Notes
    Sleep    2    # Wait for app to launch
    
    # Find and control the app
    ${app} =    Find Application    Notes
    Maximize Window    ${app}
    
    # Type some text
    Input Text    This is a demo using RPALite on macOS.
    
    # Use keyboard shortcuts
    Send Keys    {VK_LWIN}s    # Command+S to save
    Send Keys    {VK_LWIN}a    # Command+A to select all
    Send Keys    {VK_LWIN}c    # Command+C to copy
    
    # Close the app
    Close App    ${app}
```

Note: When using RPALite with Robot Framework, use `{VK_LWIN}` to represent the Command key on macOS. 
Common macOS keyboard shortcuts can be sent using combinations like:
- `{VK_LWIN}c` - Command+C (Copy)
- `{VK_LWIN}v` - Command+V (Paste)
- `{VK_LWIN}s` - Command+S (Save)
- `{VK_LWIN}w` - Command+W (Close window)

## Contribution Guidelines

If you wish to contribute code to RPALite, feel free to submit a Pull Request. Ensure your code style is consistent with the existing codebase and passes all tests in the tests directory. Additionally, make sure to update unit tests for any new or modified code.

- GitHub Repository: https://github.com/jieliu2000/RPALite
- Gitee Repository: https://gitee.com/jieliu2000/rpalite
- Gitcode Repository: https://gitcode.com/jieliu2000/rpalite
