# rnDemo

[![React Native](https://img.shields.io/badge/React_Native-0.73.0-20232A?logo=react&logoColor=61DAFB)](https://reactnative.dev)
[![React](https://img.shields.io/badge/React-18.2.0-20232A?logo=react&logoColor=61DAFB)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0.4-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Node](https://img.shields.io/badge/Node-%3E%3D18-339933?logo=node.js&logoColor=white)](https://nodejs.org)

基于 React Native 0.73 官方模板的最小示例应用。当前只有一个界面：粉色背景上有一行 **Drag this box!**，以及一个可以用手指拖动的蓝色方块。仓库里没有其它业务页面、接口或数据层。

界面逻辑全部写在根组件 [`App.tsx`](App.tsx)。它用 `PanResponder` 接管触摸，用 `Animated.ValueXY` 把位移写到方块的 `translateX` / `translateY` 上；松手时调用 `pan.extractOffset()`，方块会停在松手的位置，下一次拖动从那里继续。外层是 `ScrollView`，手势的 capture / termination 回调都写成了始终接管触摸。

仓库没有界面截图，只有 Android 启动图标（`android/app/src/main/res/mipmap-*`）。

## 当前界面

| 元素 | 实现 |
| --- | --- |
| 背景 | `ScrollView`，`backgroundColor: 'pink'`，内容居中 |
| 文案 | `Text`，内容为 `Drag this box!`，14px 加粗 |
| 方块 | 150×150、蓝色、圆角 5，包在 `Animated.View` 里 |
| 拖拽 | `PanResponder` + `Animated.event` 绑定 `dx` / `dy` |
| 松手 | `pan.extractOffset()`，位移累加保留 |

应用显示名是 `rnDemo`（[`app.json`](app.json)、Android `strings.xml`、iOS `CFBundleDisplayName`）。Android 包名 `com.rndemo`，`versionCode` 1，`versionName` 1.0。iOS `MARKETING_VERSION` 为 1.0，Bundle Identifier 为 `org.reactjs.native.example.rnDemo`。

## 技术栈

版本取自 [`package.json`](package.json) 与 [`yarn.lock`](yarn.lock) 中已锁定的结果。

| 类别 | 版本 |
| --- | --- |
| React Native | 0.73.0（CLI 12.1.1） |
| React / react-test-renderer | 18.2.0 |
| TypeScript | 5.0.4 |
| Metro 配置 | `@react-native/metro-config` 0.73.2 |
| Babel | `@babel/core` ^7.20.0，`@react-native/babel-preset` 0.73.18 |
| Jest | 29.7.0（`react-native` preset） |
| ESLint | 8.56.0（`@react-native/eslint-config` 0.73.1） |
| Prettier | 2.8.8 |
| JS 引擎 | Hermes（`android/gradle.properties` 中 `hermesEnabled=true`） |
| 新架构 | 关闭（`newArchEnabled=false`） |
| 调试 | 模板自带的 Flipper 集成仍在 Android / iOS 工程里 |

声明了但**没有任何源码引用**的依赖：

- `react-native-snap-carousel` 3.9.1
- `@types/react-native-snap-carousel` 3.8.9

当前界面没有轮播组件。

### Android

| 项 | 值 |
| --- | --- |
| Gradle Wrapper | 8.3 |
| Android Gradle Plugin | 8.1.1（由 `@react-native/gradle-plugin` 0.73.4 带入） |
| Kotlin | 1.8.0 |
| compileSdk / targetSdk / buildTools | 34 / 34 / 34.0.0 |
| minSdk | 21 |
| NDK | 25.1.8937393 |
| ABI | `armeabi-v7a`、`arm64-v8a`、`x86`、`x86_64` |

### iOS

| 项 | 值 |
| --- | --- |
| 最低系统 | iOS 13.4 |
| CocoaPods | `Gemfile` 要求 `~> 1.13`；`Podfile.lock` 由 1.14.3 生成 |
| Ruby | `>= 2.6.10` |
| 依赖安装路径 | `.bundle/config` 指定 `vendor/bundle` |

## 目录结构

```text
.
├── App.tsx                 # 唯一界面：可拖拽方块
├── index.js                # AppRegistry 注册根组件 rnDemo
├── app.json                # name / displayName
├── __tests__/App.test.tsx  # Jest：渲染 App，不断言具体文案
├── android/                # Android 原生工程（Kotlin，applicationId com.rndemo）
├── ios/                    # iOS 原生工程（rnDemo scheme，含 rnDemoTests）
├── package.json            # 脚本与依赖；要求 Node >= 18
├── yarn.lock               # 请用 Yarn 安装，仓库没有 package-lock.json
├── babel.config.js
├── metro.config.js         # 默认 Metro 配置，无自定义项
├── jest.config.js
├── tsconfig.json           # 继承 @react-native/typescript-config
├── Gemfile                 # CocoaPods
└── .npmrc                  # registry=https://registry.npm.taobao.org/
```

## 环境要求

- Node.js **>= 18**（`package.json` 的 `engines`）
- Yarn 1（仓库使用 `yarn.lock`；本文验证时为 Yarn 1.22.22）
- Android：Android SDK、可用的模拟器或真机。React Native 0.73 的官方环境说明使用 **JDK 17**
- iOS：macOS、Xcode，以及 CocoaPods（可用仓库自带的 `Gemfile`）

本仓库的 `.npmrc` 把 registry 指到 `https://registry.npm.taobao.org/`。`yarn.lock` 里的包地址是 `registry.npmmirror.com`。

## 安装与运行

在仓库根目录执行。

```bash
yarn install --frozen-lockfile
```

启动 Metro：

```bash
yarn start
```

另开一个终端，从仓库根目录启动原生应用。

```bash
# Android（需要已启动的模拟器或已连接的真机）
yarn android

# iOS（仅 macOS；首次需要先安装 Pods）
bundle install
cd ios && bundle exec pod install && cd ..
yarn ios
```

也可以用 Android Studio 打开 `android/`，或用 Xcode 打开 `ios/rnDemo.xcworkspace` 直接运行。改完 `App.tsx` 后，Android 在开发者菜单里选 Reload（Linux / Windows 为 Ctrl+M），iOS 模拟器使用 Cmd+R。

### 脚本

| 命令 | 作用 |
| --- | --- |
| `yarn start` | 启动 Metro |
| `yarn android` | `react-native run-android` |
| `yarn ios` | `react-native run-ios` |
| `yarn test` | Jest，当前只有渲染用例 |
| `yarn lint` | ESLint（含 Prettier 规则） |

### 本次验证

在 Linux 环境（Node v22.23.4、Yarn 1.22.22）中：

- `yarn install --frozen-lockfile` 成功
- `yarn test --watchAll=false` 通过：1 个测试套件、1 个用例
- `yarn lint` **未通过**。`App.tsx` 的 import 换行和文件末尾换行不符合 Prettier 规则（2 个 error、1 个 warning）。测试运行时还有一条 `Animated.event now requires a second argument for options` 警告，用例仍然通过
- 未运行 `yarn android` / `yarn ios`：当前环境没有 Android SDK 与模拟器，也无法构建 iOS

## 备注

- `ios/rnDemoTests/rnDemoTests.m` 仍是模板测试，它查找无障碍文案 `Welcome to React`。当前界面只有 `Drag this box!`，没有这句文案，因此这条 XCTest 与现有界面不一致。
- `react-native-snap-carousel` 只出现在依赖声明里，入口和 `App.tsx` 都没有引用它。
- Release 的 Android 签名仍使用 debug keystore，并关闭了 ProGuard（`enableProguardInReleaseBuilds = false`）。正式发布前需要换成自己的签名配置。
- 仓库没有 `LICENSE` 文件。

## 参考

- [React Native 环境搭建](https://reactnative.dev/docs/environment-setup)
- [React Native 入门](https://reactnative.dev/docs/getting-started)
- [PanResponder](https://reactnative.dev/docs/panresponder)
- [Animated](https://reactnative.dev/docs/animated)
