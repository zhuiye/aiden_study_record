# RN 中 保存图片

在 APP 中保存图片,需要做很多处理,首先,我们先考虑图片的来源,大致有如下格式的图片:

- url

  网络图片

- base64

  某些工具生成的 base64 格式的图片

- file://

  本地的图片

对此 我们需要用到 的库如下:

- [react-native-fs](https://github.com/itinance/react-native-fs/)
- [react-native-community/cameraroll](https://github.com/react-native-community/react-native-cameraroll)

- [react-native-permissions](https://github.com/react-native-community/react-native-permissions)

## react-native-community/cameraroll

### save

保存图片,我们主要关注 save 方法

```ts
CameraRoll.save(tag, { type, album });
```

> On Android, the tag must be a local image or video URI, such as "file:///sdcard/img.png".

在 android 上,tag 必须为 本地图片 或者 视频的 URL

> On iOS, the tag can be any image URI (including local, remote asset-library and base64 data URIs) or a local video file URI (remote or data URIs are not supported for saving video at this time).

在 iso 上,tag 可以是一个 image URI ,包括 本地,远程资源图片,以及 base64 图片 和一个 本地的 视频文件 URI (暂不支持 保存 远程 或者 数据 URLs 的视频 )

可以看到,上面的描述 还是 比较坑,就没有一个统一方案,能保存开头列举的三种格式的图片,Android 上需要保存 网络图片和 base64 图片的库 就需要借助 react-native-fs

## react-native-fs

此库我们在此处主要用于从网络中下载图片,把 base64 图片存放在 临时目录下,然后存放在 相册中

## react-native-permissions

此库主要用于权限处理的库

## 实现

```ts
import { Platform } from "react-native";
import RNFS from "react-native-fs";
import CameraRoll from "@react-native-community/cameraroll";
import { PERMISSIONS } from "react-native-permissions";
import { ensurePermissions, requestOpenSetting } from "./permissions";

export const base64StringHeader = "data:image/png;base64,";

export async function saveImage(uri: string) {
  if (Platform.OS === "ios") {
    return CameraRoll.save(uri, { type: "photo" });
  } else {
    const permissions = [PERMISSIONS.ANDROID.WRITE_EXTERNAL_STORAGE];
    const granted = await ensurePermissions(permissions);
    if (!granted) {
      requestOpenSetting();
      throw Error("权限不足");
    }

    if (uri.startsWith("file://")) {
      return CameraRoll.save(uri);
    }

    const dlDest = `${RNFS.TemporaryDirectoryPath}/dowload-${Date.now()}.png`;

    if (uri.startsWith(base64StringHeader)) {
      const data = uri.split(base64StringHeader)[1];
      await RNFS.writeFile(dlDest, data, "base64");
    } else {
      await RNFS.downloadFile({ fromUrl: uri, toFile: dlDest }).promise;
    }
    return CameraRoll.save("file://" + dlDest, { type: "photo" });
  }
}
```
