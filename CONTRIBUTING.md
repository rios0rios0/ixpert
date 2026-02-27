# Contributing

> **This project was discontinued in March 2012 and is no longer actively maintained.**
> The repository is preserved as a historical reference. No new features or bug fixes are planned.

## Historical Build Information

This project was built using the following tools and technologies:

- **Language:** Objective-C (manual reference counting / MRC)
- **Platform:** iOS (iPhone/iPod), iOS SDK 5.x
- **Frameworks:** UIKit, CommonCrypto (CC_MD5), Foundation
- **IDE:** Xcode 4.x with Interface Builder (XIB/NIB files)

### Build Steps (Historical)

1. Open `Project/iXpert.xcodeproj` in Xcode (originally Xcode 4.x)
2. Select an iPhone Simulator target
3. Build and run (`Cmd+R`)

> **Note:** Modern Xcode versions may require adding `-fno-objc-arc` compiler flags since the project uses manual reference counting.
