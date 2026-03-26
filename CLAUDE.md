# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

### Main Library
- `swift build` — Build SwiftPM library target
- `xcodebuild -scheme JXPhotoBrowser -project JXPhotoBrowser.xcodeproj build` — Build main framework

### Demos
- `cd Demo-UIKit && pod install` — Install UIKit demo dependencies (CocoaPods)
- `xcodebuild -scheme Demo -project Demo-UIKit/Demo.xcodeproj build` — Build UIKit demo
- `xcodebuild -scheme Demo -project Demo-SwiftUI/Demo.xcodeproj build` — Build SwiftUI demo
- `cd Demo-Carthage && carthage update --use-xcframeworks --platform iOS` — Carthage dependencies

Note: When using CocoaPods, open `Demo.xcworkspace` (not `.xcodeproj`) after running `pod install`.

## Architecture

JXPhotoBrowser is a lightweight iOS image/video browser built around a single kernel with protocol-based extension points.

### Core Components
- `JXPhotoBrowserViewController` — Browser kernel using `UICollectionView` for pagination and reuse
- `JXZoomImageCell` — Scalable image cell with `UIScrollView` for pinch/double-tap zoom
- `JXImageCell` — Lightweight cell for embedded banner scenarios (no zoom)
- `JXPhotoBrowserCellProtocol` — Minimal 2-property protocol (`browser`, `transitionImageView`) for custom cells
- `JXPhotoBrowserDelegate` — Provides item count, cell instances, lifecycle callbacks, and thumbnail for Zoom transition
- `JXPhotoBrowserOverlay` — Plugin protocol for附加 UI (page indicators, close buttons, etc.)
- `JXPageIndicatorOverlay` — Built-in page indicator based on `UIPageControl`

### Animators
- `JXZoomPresentAnimator` / `JXZoomDismissAnimator` — Zoom transition (微信-style)
- `JXFadeAnimator` — Classic fade transition
- `JXNoneAnimator` — No transition

### Key Design
- **Zero data model dependency**: Browser delegates data loading to host via `JXPhotoBrowserDelegate`
- **Virtual data source for looping**: `realCount * loopMultiplier` mapping keeps scroll direction continuous
- **Overlay mechanism**: Optional UI components loaded on-demand via `addOverlay()`
- **Same kernel for fullscreen and banner**: `PhotoBannerView` reuses the browser controller with `transitionType = .none`

### Source File Guide
| File | Role |
|------|------|
| `Sources/JXPhotoBrowserViewController.swift` | Browser kernel, pagination, looping, overlay management, pan-to-dismiss |
| `Sources/JXZoomImageCell.swift` | Single-page zoom with double-tap toggle and aspect-fit centering |
| `Sources/JXZoomPresentAnimator.swift` | Zoom present transition with Fade fallback when thumbnails unavailable |
| `Demo-UIKit/Demo/HomePage/VideoPlayerCell.swift` | Custom cell extending `JXZoomImageCell` for video playback |
| `Demo-UIKit/Demo/HomePage/PhotoBannerView.swift` | Banner reusing browser kernel with `JXImageCell` |

## Code Style
- 4-space indentation
- `// MARK:` for section organization
- Public types prefixed with `JX` (e.g., `JXPhotoBrowserViewController`)
- No SwiftLint/SwiftFormat configured — match surrounding code style

## Testing
No independent `Tests/` target exists. Before committing, build the main library and affected demo targets, then manually verify key interactions (zoom, pan-to-dismiss, looping, SwiftUI bridging).
