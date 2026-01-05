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

对于 ios 应用，修改 [文件](../Gemfile), 添加 `gem 'nkf' # LoadError - cannot load such file -- kconv`