<h1 align="center">iXpert</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/ixpert/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/ixpert.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/ixpert/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/ixpert.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

An iOS (iPhone/iPod) application for encoding and decoding computational data formats, built with Objective-C. The app provides nine conversion modes covering numeral systems, text encoding, hashing, and string manipulation, all presented through animated view transitions with custom-designed button graphics. Development discontinued on 2012-03-09.

## Features

### Conversion Modes

| Mode | Encode | Decode | Description |
|------|--------|--------|-------------|
| **Hex** (ASCII) | ASCII to Hex | Hex to ASCII | Character-by-character hexadecimal encoding of text |
| **Hex2** (Decimal) | Decimal to Hex | Hex to Decimal | Numeric base conversion between decimal and hexadecimal |
| **Bin** (ASCII) | ASCII to Binary | Binary to ASCII | Character-by-character binary encoding of text |
| **Bin2** (Decimal) | Decimal to Binary | Binary to Decimal | Numeric base conversion between decimal and binary |
| **Oct** (Decimal) | Decimal to Octal | Octal to Decimal | Numeric base conversion between decimal and octal |
| **Rev** (Reverse) | Reverse string | -- | Reverses the input string (one-way, no decode) |
| **Loo** (Leet/Loco) | ASCII to Leet | Leet to ASCII | Leet-speak text transformation |
| **B64** (Base64) | Base64 encode | Base64 decode | Standard Base64 encoding/decoding |
| **MD5** | MD5 hash | -- | One-way MD5 hash generation (no decode button) |

### User Interface
- **Option view** -- main menu with 9 conversion mode buttons and a "How to Use" button
- **Detail view** -- input/output text views with Encode and Decode buttons, switching via flip animation
- **How-to view** -- usage instructions, revealed with a curl-down page animation
- **Custom input accessory view** -- a "Done" button above the keyboard for dismissing it
- **Input validation** -- restricts keyboard input based on the selected mode (e.g., hex characters only for Hex2, digits only for Bin2 and Oct)
- **Animated view transitions** -- `UIViewAnimationTransitionFlipFromRight/Left` for option/detail switching, `CurlDown/Up` for how-to pages

### Architecture
- **`MasterViewController`** -- root controller managing view switching between Option, Detail, and How-to views
- **`OptionViewController`** -- displays the 9 conversion mode buttons and "More" action sheet
- **`DetailViewController`** -- handles encoding/decoding with input validation and keyboard management
- **`HowtoViewController`** -- displays usage instructions
- **`RatingSystem`** -- static class implementing all numeral system conversions (Hex, Binary, Octal, Reverse, Leet)
- **`Hashes`** -- static class implementing Base64 encoding/decoding and MD5 hashing

## Technologies

- **Objective-C** (manual reference counting)
- **Xcode** with Interface Builder (XIB/NIB files)
- **UIKit** -- `UITextView`, `UIButton`, `UIImageView`, `UIActionSheet`, `UIAlertView`
- **CommonCrypto** -- MD5 hash generation (`CC_MD5`)
- **Foundation** -- `NSCharacterSet` for input filtering
- **iOS SDK 5.x** (iPhone deployment target)

## Project Structure

```
ixpert/
├── Build/
│   └── iXpert.app/                      # Pre-built application bundle with all assets
├── Imgs/
│   ├── AccessoryView/                   # Keyboard accessory bar background
│   ├── Backgrounds/                     # Per-mode background images (Hex, Bin, Oct, B64, MD5, etc.)
│   ├── Buttons/                         # Normal/selected states for all mode buttons + navigation
│   ├── Default/                         # Splash screen images
│   └── Icons/                           # App icons in multiple sizes + PSD sources
├── Project/
│   ├── iXpert.xcodeproj/
│   └── iXpert/
│       ├── AppDelegate.{h,m}
│       ├── MasterViewController.{h,m}   # Root controller with view switching logic
│       ├── OptionViewController.{h,m}   # Mode selection menu
│       ├── DetailViewController.{h,m}   # Encoding/decoding interface with input validation
│       ├── HowtoViewController.{h,m}    # Usage instructions
│       ├── RatingSystem.{h,m}           # Numeral system conversion algorithms
│       ├── Hashes.{h,m}                 # Base64 and MD5 implementations
│       ├── main.m
│       └── *.png                        # Runtime image assets
└── LICENSE
```

## Installation

1. Install **Xcode** (originally developed with Xcode 4.x)
2. Open `Project/iXpert.xcodeproj`
3. Select an iPhone Simulator target and run (Cmd+R)

> **Note:** This project uses manual reference counting (MRC). Modern Xcode may require `-fno-objc-arc` compiler flags.

## Usage

1. Launch the app -- the option screen appears with 9 conversion mode buttons
2. Tap a mode button (e.g., **Hex**, **B64**, **MD5**) -- the view flips to the detail screen
3. Enter text in the input field
4. Tap **Encode** to convert, or **Decode** to reverse the conversion (where applicable)
5. Tap the **Back** button to flip back to the mode selection
6. Tap the **"i"** button for "More Apps" and "About This App" information

## Contributing

This project is no longer actively maintained. You may submit small bug fixes or documentation improvements via Pull Request, but reviews may be infrequent and contributions are not guaranteed to be merged.

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.
