# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

iXpert is a **discontinued (2012-03-09) iOS app** for encoding/decoding computational data formats, written in Objective-C. The repository is preserved as a historical reference — no new features or fixes are planned.

## Critical convention

- The project uses **manual reference counting (MRC)**, not ARC. Code calls `retain`/`release`/`autorelease` and `dealloc` releases ivars directly.
- Never introduce ARC constructs (`@autoreleasepool`, `__strong`/`__weak`, or removing `release` calls). Modern Xcode needs `-fno-objc-arc` per file or ARC disabled for the target.

## Build / test / lint

- There are **no build, test, lint, or CI commands** — all work was done in Xcode. No GitHub Actions, no test suite (the project predates XCTest).
- Open in Xcode (originally 4.x): `open Project/iXpert.xcodeproj`, select an iPhone Simulator target, `Cmd+R`.

## Architecture

- `MasterViewController` is the root controller. It owns `OptionViewController`, `DetailViewController`, and `HowtoViewController` and swaps them in/out manually via `removeFromSuperview` + `insertSubview:atIndex:` — there is no `UINavigationController`. View transitions are animated with `UIViewAnimationTransitionFlipFromRight/Left` (option ↔ detail) and `CurlDown/Up` (option ↔ how-to).
- `MasterViewController ChangeViews:` is a single `IBAction` switching on the sender button to set the mode string (`NSStringOption`), background image, and keyboard type before flipping. Adding a mode means extending that branch plus `viewDidLoad`'s button wiring.
- Conversion logic lives in two static utility classes (class `+` methods only, no instances): `RatingSystem` (Hex/Bin/Oct/Reverse/Leet, both numeral-system and ASCII variants) and `Hashes` (Base64 encode/decode, MD5 via CommonCrypto). Naming follows `XtoY:` (e.g. `ASCIItoHex:`, `HextoDec:`).
- Input validation lives in `DetailViewController` using `NSCharacterSet` invertedSet filtering per mode (plus `keyboardType` set by `MasterViewController` — `UIKeyboardTypeNumberPad` for Bin2/Oct).
- One-way modes (Rev, MD5) hide the Decode button (`UIButtonDec.hidden = YES`).

## Notes

- UI is built in Interface Builder `.xib` files; image assets are bundled as PNGs in `Project/iXpert/`, with higher-res sources in `Imgs/`.
- `Build/iXpert.app` and `Build/iXpert.ipa` are committed as historical artifacts.
- `.github/copilot-instructions.md` covers the same ground in more detail (mode tables, full file map, tech stack); consult it before broad searches.
</content>
</invoke>
