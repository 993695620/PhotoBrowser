# Repository Guidelines

## 项目结构与模块划分
`Sources/` 是正式发布的库源码，包含浏览器核心类型、转场动画、图片 Cell、Overlay 组件以及 `PrivacyInfo.xcprivacy`。`Demo-UIKit/` 是基于 CocoaPods 的 UIKit 示例，`Demo-SwiftUI/` 展示 SwiftUI 桥接用法，`Demo-Carthage/` 用于演示 Carthage 集成。根目录下的 `Package.swift`、`JXPhotoBrowser.podspec`、`README*.md` 和 `CHANGELOG.md` 分别负责包管理、发布配置和文档说明。不要手动修改 `Demo-UIKit/Pods/`、`.build/` 等生成目录。

## 构建、测试与开发命令
按你正在修改的集成方式选择命令：

- `swift build`：构建 SwiftPM 库目标。
- `xcodebuild -scheme JXPhotoBrowser -project JXPhotoBrowser.xcodeproj build`：直接构建主框架工程。
- `cd Demo-UIKit && pod install`：安装 UIKit 示例依赖，再打开 `Demo.xcworkspace`。
- `xcodebuild -scheme Demo -project Demo-UIKit/Demo.xcodeproj build`：构建 UIKit 示例。
- `xcodebuild -scheme Demo -project Demo-SwiftUI/Demo.xcodeproj build`：构建 SwiftUI 示例。
- `cd Demo-Carthage && carthage update --use-xcframeworks --platform iOS`：首次准备 Carthage 示例依赖。

## 代码风格与命名约定
遵循现有 Swift 风格：4 空格缩进，使用 `// MARK:` 分段，公开类型保持 `JX` 前缀。类型名使用大驼峰，如 `JXPhotoBrowserViewController`；属性和方法使用小驼峰，如 `isLoopingEnabled`。扩展应按职责拆分，尽量保持单一功能。仓库未提交 `SwiftLint` 或 `SwiftFormat` 配置，因此修改时以周边文件风格为准。

## 测试说明
当前仓库没有独立的 `Tests/` 目标。提交前至少应构建主库以及受影响的示例工程，并在模拟器中手动验证关键交互，尤其是缩放、下拉关闭、循环滚动和 SwiftUI 桥接场景。
