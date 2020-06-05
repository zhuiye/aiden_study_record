## React Native Linking

## android 创建深层次链接

也就是自定义的链接,如 :'myapp://hengcheng.com/id=aa?'

https://developer.android.com/training/app-links/deep-linking#handling-intents

在 react native 中 可以

```js
Linking.getInitialURL().then(
  (url: (string) => {
    // 获取  "myapp://hengcheng.com/id=aa?"
    // 接下来处理 这个链接
  })
);
```

https://reactnative.dev/docs/linking

iOS 开启深层度链接

https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content?language=objc
