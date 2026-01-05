# 构建项目说明

执行构建命令如下：

```bash
npx @react-native-community/cli@latest init rn_demo_083
yarn add react-native-chat-sdk
yarn add react-native-chat-uikit
yarn add @react-native-async-storage/async-storage @react-native-camera-roll/camera-roll @react-native-clipboard/clipboard @react-native-documents/picker react-native-audio-recorder-playerr@~3.6.12 react-native-create-thumbnail  react-native-device-info react-native-file-access react-native-gesture-handler react-native-image-picker react-native-video
bundle install
cd ios && bundle exec pod install
```

对于 ios 应用的问题: 修改 [文件](../Gemfile), 添加 `gem 'nkf' # LoadError - cannot load such file -- kconv`

对于 android 应用的问题: 0.83 版本 和 react-native-audio-recorder-player@3.6.x 版本 的兼容问题。

错误提示:
```log
e: file:///Users/asterisk/tmp2025/2025-03-18/tmp/rn_demo_083/node_modules/react-native-audio-recorder-player/android/src/main/java/com/dooboolab.audiorecorderplayer/RNAudioRecorderPlayerModule.kt:47:56 Unresolved reference 'currentActivity'.
e: file:///Users/asterisk/tmp2025/2025-03-18/tmp/rn_demo_083/node_modules/react-native-audio-recorder-player/android/src/main/java/com/dooboolab.audiorecorderplayer/RNAudioRecorderPlayerModule.kt:53:56 Unresolved reference 'currentActivity'.
e: file:///Users/asterisk/tmp2025/2025-03-18/tmp/rn_demo_083/node_modules/react-native-audio-recorder-player/android/src/main/java/com/dooboolab.audiorecorderplayer/RNAudioRecorderPlayerModule.kt:255:49 Unresolved reference 'currentActivity'.
e: file:///Users/asterisk/tmp2025/2025-03-18/tmp/rn_demo_083/node_modules/react-native-audio-recorder-player/android/src/main/java/com/dooboolab.audiorecorderplayer/RNAudioRecorderPlayerModule.kt:255:67 Unresolved reference 'applicationContext'.
```
将 `currentActivity!!.applicationContext` 改为 `reactContext.applicationContext`：

```bash
# 将修改的内容放在补丁文件中
yarn patch react-native-audio-recorder-player
# 将补丁应用到 package.json 配置文件
yarn patch-commit -s /private/var/folders/_x/3sg_ghyj03q5_xhxffbhy2k00000gp/T/xfs-fc5f5734/user
```
