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

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body "<imperative description>"`
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body "<description>"`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
