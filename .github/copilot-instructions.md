# iXpert Repository

**ALWAYS FOLLOW THESE INSTRUCTIONS FIRST.** Only fallback to additional search and context gathering if the information in these instructions is incomplete or found to be in error.

iXpert is a discontinued (2012-03-09) iOS (iPhone/iPod) application for encoding and decoding computational data formats, built with Objective-C using manual reference counting. The repository is preserved as a **historical reference only** — no new features or bug fixes are planned.

## Quick Reference

**Build Prerequisites:**
- Xcode 4.x (originally; modern Xcode requires `-fno-objc-arc` flags)
- iPhone Simulator target

**Opening the project:**
```bash
open Project/iXpert.xcodeproj
# Then: select an iPhone Simulator target and press Cmd+R
```

> **Note:** There are no automated build, test, lint, or CI/CD commands. All development was done entirely within Xcode.

## Working Effectively

### Opening the Project
1. Open `Project/iXpert.xcodeproj` in Xcode (originally developed with Xcode 4.x)
2. Select an iPhone Simulator target (iPhone family)
3. Build and run with `Cmd+R`

### Manual Reference Counting (MRC)
- The project uses **manual retain/release** (`retain`, `release`, `autorelease`) — **not ARC**
- Modern Xcode: set the `-fno-objc-arc` compiler flag per file, or disable ARC for the whole target
- Never add `@autoreleasepool` blocks or `__strong`/`__weak` qualifiers — these are ARC constructs

### Objective-C Conventions Used
- Class interfaces in `.h`, implementations in `.m`
- UI layouts in Interface Builder `.xib` files
- Static utility classes with `+` (class-level) methods only: `RatingSystem`, `Hashes`
- View controllers manage their own view lifecycle and delegate callbacks directly

## Architecture

```
MasterViewController  (root controller)
├── OptionViewController   (9-button mode selection menu)
├── DetailViewController   (encode/decode interface + input validation)
└── HowtoViewController    (usage instructions page)

Utility classes:
├── RatingSystem    (Hex, Binary, Octal, Reverse, Leet conversions)
└── Hashes          (Base64 encode/decode, MD5 hashing via CommonCrypto)
```

### View Transition Animations
- Option ↔ Detail: `UIViewAnimationTransitionFlipFromRight/Left`
- Detail ↔ How-to: `UIViewAnimationTransitionCurlDown/Up`

### Conversion Modes

| Mode        | Encode              | Decode              |
|-------------|---------------------|---------------------|
| Hex (ASCII) | ASCII → Hex         | Hex → ASCII         |
| Hex2 (Dec)  | Decimal → Hex       | Hex → Decimal       |
| Bin (ASCII) | ASCII → Binary      | Binary → ASCII      |
| Bin2 (Dec)  | Decimal → Binary    | Binary → Decimal    |
| Oct (Dec)   | Decimal → Octal     | Octal → Decimal     |
| Rev         | Reverse string      | *(one-way)*         |
| Loo (Leet)  | ASCII → Leet        | Leet → ASCII        |
| B64         | Base64 encode       | Base64 decode       |
| MD5         | MD5 hash            | *(one-way)*         |

## Technology Stack

| Component       | Details                                      |
|-----------------|----------------------------------------------|
| Language        | Objective-C (manual reference counting)      |
| IDE             | Xcode 4.x with Interface Builder (XIB/NIB)   |
| Platform        | iOS 5.x — iPhone/iPod deployment target      |
| UI Framework    | UIKit (`UITextView`, `UIButton`, `UIImageView`, `UIActionSheet`, `UIAlertView`) |
| Crypto          | CommonCrypto — `CC_MD5` for MD5 hashing      |
| Encoding        | Foundation — `NSCharacterSet`, `NSData` for Base64 |

## Repository Structure

```
ixpert/
├── .github/
│   └── copilot-instructions.md   # This file
├── Build/
│   ├── iXpert.app/               # Pre-built application bundle (historical artifact)
│   └── iXpert.ipa                # Pre-built IPA (historical artifact)
├── Imgs/
│   ├── AccessoryView/            # Keyboard accessory bar background
│   ├── Backgrounds/              # Per-mode background images (Hex, Bin, Oct, B64, MD5, …)
│   ├── Buttons/                  # Normal/selected states for all mode buttons + navigation
│   ├── Default/                  # Splash screen images
│   └── Icons/                    # App icons in multiple sizes + PSD sources
├── Project/
│   ├── iXpert.xcodeproj/         # Xcode project file
│   └── iXpert/
│       ├── AppDelegate.{h,m}
│       ├── MasterViewController.{h,m}   # Root controller with view switching logic
│       ├── OptionViewController.{h,m}   # Mode selection menu
│       ├── DetailViewController.{h,m}   # Encoding/decoding interface + input validation
│       ├── HowtoViewController.{h,m}    # Usage instructions
│       ├── RatingSystem.{h,m}           # Numeral system conversion algorithms
│       ├── Hashes.{h,m}                 # Base64 and MD5 implementations
│       ├── main.m
│       ├── *.xib                        # Interface Builder layout files
│       └── *.png                        # Runtime image assets
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Key Files

| File | Purpose |
|------|---------|
| `Project/iXpert/RatingSystem.{h,m}` | All numeral system conversions (Hex, Bin, Oct, Rev, Leet) |
| `Project/iXpert/Hashes.{h,m}` | Base64 and MD5 implementations |
| `Project/iXpert/DetailViewController.{h,m}` | Encode/Decode UI, input validation, keyboard handling |
| `Project/iXpert/MasterViewController.{h,m}` | Root controller managing animated view transitions |
| `Project/iXpert/OptionViewController.{h,m}` | 9-button main menu and "More" action sheet |
| `Project/iXpert.xcodeproj/project.pbxproj` | Xcode project configuration |

## CI/CD

This project has **no CI/CD pipeline**. There are no GitHub Actions workflows, automated tests, linters, or deployment scripts. The repository is a historical archive.

## Development Notes

- **No tests exist** — the project predates widespread iOS unit testing adoption (XCTest was introduced in Xcode 5)
- **Input validation** is done in `DetailViewController` using `NSCharacterSet` to restrict keyboard input per mode (e.g., hex characters for Hex2, digits for Bin2 and Oct)
- **Image assets** are bundled directly in `Project/iXpert/` as PNG files; higher-resolution assets live in `Imgs/`
- **Pre-built artifacts** (`Build/iXpert.app`, `Build/iXpert.ipa`) are committed to the repository as historical artifacts
