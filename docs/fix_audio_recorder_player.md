# 修复 react-native-audio-recorder-player 兼容性问题

## 问题描述

在 React Native 0.83 项目中使用 `react-native-audio-recorder-player@3.6.x` 版本时，运行 `yarn run android` 编译 Android 应用会报错：

```
> Task :react-native-audio-recorder-player:compileDebugKotlin FAILED

e: Unresolved reference 'currentActivity'.
e: Unresolved reference 'applicationContext'.
```

## 问题原因

React Native 0.83 采用了新架构，`ReactContextBaseJavaModule` 的 API 发生了变化：

- 在旧版本中，可以直接在模块类中使用 `currentActivity` 和 `applicationContext` 属性
- 在新架构中，需要通过 `reactApplicationContext` (或构造函数中的 `reactContext`) 来访问这些属性

`react-native-audio-recorder-player@3.6.x` 是旧版本，使用了旧架构的 API，导致与 React Native 0.83 不兼容。

## 解决方案

### 方案一：升级到 4.x 版本（可能引入其他依赖）

```bash
yarn add react-native-audio-recorder-player@^4.5.0
```

> 注意：4.x 版本可能需要安装额外的依赖项。

### 方案二：使用 Yarn Patch 修复 3.6.x 版本（推荐）

使用 Yarn 4 内置的 patch 功能修复源码：

#### 1. 创建补丁

```bash
yarn patch react-native-audio-recorder-player
```

#### 2. 修改临时目录中的源文件

编辑 `android/src/main/java/com/dooboolab.audiorecorderplayer/RNAudioRecorderPlayerModule.kt`：

将所有 `currentActivity` 改为 `reactContext.currentActivity`：

```kotlin
// 修改前
ActivityCompat.requestPermissions((currentActivity)!!, ...)

// 修改后
ActivityCompat.requestPermissions((reactContext.currentActivity)!!, ...)
```

将 `currentActivity!!.applicationContext` 改为 `reactContext.applicationContext`：

```kotlin
// 修改前
mediaPlayer!!.setDataSource(currentActivity!!.applicationContext, Uri.parse(path), headers)

// 修改后
mediaPlayer!!.setDataSource(reactContext.applicationContext, Uri.parse(path), headers)
```

#### 3. 提交补丁

```bash
yarn patch-commit -s <临时目录路径>
```

补丁文件会自动保存到 `.yarn/patches/` 目录，并在 `package.json` 中记录。

## 补丁文件位置

```
.yarn/patches/react-native-audio-recorder-player-npm-3.6.14-3e27200e10.patch
```

## 验证修复

运行以下命令验证编译是否成功：

```bash
cd android && ./gradlew :react-native-audio-recorder-player:compileDebugKotlin
```

如果显示 `BUILD SUCCESSFUL`，则修复成功。

## 相关信息

- React Native 版本：0.83.1
- react-native-audio-recorder-player 版本：3.6.14
- Yarn 版本：4.11.0
- 修复日期：2026-01-05
