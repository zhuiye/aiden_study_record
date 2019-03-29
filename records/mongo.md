# typescript In react Native

https://github.com/Microsoft/TypeScript-React-Native-Starter

https://github.com/techird/blog/issues/3

## first

```
yarn add --dev typescript
yarn add --dev react-native-typescript-transformer
yarn tsc --init --pretty --jsx react
touch rn-cli.config.js
yarn add --dev @types/react @types/react-native
```

## second

tsconfig.json文件下解开**allowSyntheticDefaultImports**的 注释

```
{
  ...
  // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
  ...
}
```

## third

rn-cli.config.js 文件下添加

```
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-typescript-transformer");
  },
  getSourceExts() {
    return ["ts", "tsx"];
  }
};
```

## Migrating to TypeScript

Rename the generated `App.js` and `__tests__/App.js` files to `App.tsx`. `index.js` needs to use the `.js` extension. All new files should use the `.tsx` extension (or `.ts` if the file doesn't contain any JSX).

If you try to run the app now, you'll get an error like `object prototype may only be an object or null`. This is caused by a failure to import the default export from React as well as a named export on the same line. Open `App.tsx` and modify the import at the top of the file:

**重新命名生成的App.js  文件为App.tsx ,而index.js 需要用.js 扩展名,此后全部的文件应该使用 .tsx 或者.ts 并且该文件中不包含JSX**

## Simple example

App.js

```tsx
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import { Platform, StyleSheet, Text, View } from 'react-native';
import Hello from './src/Hello';

type Props = {};
export default class App extends Component<Props> {
  render() {
    return (
      <View style={styles.container}>
        <Hello name="hengcheng" />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF'
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5
  }
});

```

Hello.tsx

```tsx
// components/Hello.tsx
import React from 'react';
import { Button, StyleSheet, Text, View } from 'react-native';

export interface Props {
  name: string;
}

interface State {}

export default class Hello extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props);
  }

  render() {
    return (
      <View>
        <Text>{this.props.name}</Text>
        <Text>Test Typescript....</Text>
      </View>
    );
  }
}

```

关于编辑器不提示样式的问题解决方法

https://github.com/Microsoft/vscode-react-native/issues/379

封装公共的styleSheet

```ts
import {
  StyleSheet as RnStyleSheet,
  ViewStyle,
  TextStyle,
  ImageStyle
} from 'react-native';

type StyleProps = Partial<ViewStyle | TextStyle | ImageStyle>;

const StyleSheet = {
  create(styles: { [className: string]: StyleProps }) {
    return RnStyleSheet.create(styles);
  },
  hairlineWidth: RnStyleSheet.hairlineWidth
};

export default StyleSheet;

```

## Question

### 1.真机 dedug remote js 调试红屏幕报错

**react native  dedug remote js getDeviceId:Neither user 10645**

解决:进入应用授权界面,给所有的权限添加允许 





## typescript 封装的简单例子

```tsx
import React from 'react';
import {
  TouchableOpacity,
  ActivityIndicator,
  ViewStyle,
  TextStyle,
  StyleSheet,
} from 'react-native';
/*
  声明传入组件的属性接口....
*/
interface ButtonProp {
  text: string;
  width?: number; //可选
  disabled?: boolean;
  waiting?: boolean;
  style?: ViewStyle | ViewStyle[]; //样式,ViewStyle //或者ViewStyle类型数组
  textStyle?: TextStyle;
  onPress?: () => void;
}
/*
	声明纯组件 :React.SFC
	
*/
export const BorderButton: React.SFC<ButtonProp> = ({
  text,
  disabled,
  waiting,
  style,
  textStyle,
  onPress,
  width = WINDOW.WIDTH - 2 * STYLE_SIZE.SPACING_XW,
}) => (
  <TouchableOpacity
    disabled={disabled || waiting}
    onPress={onPress}
    activeOpacity={0.6}
    style={[
      styles.buttonBase,
      { width },
      styles.borderBtn,
      style,
      disabled ? styles.borderDisabled : null,
    ]}
  >
    {waiting ? (
      <ActivityIndicator
        size={STYLE_SIZE.FONT_BUTTON_TEXT}
        color={STYLE_COLOR.THEME_BLUE}
      />
    ) : (
      <MText style={[styles.borderText, textStyle]}>{text}</MText>
    )}
  </TouchableOpacity>
);
```

## VSCode  插件

https://zhuanlan.zhihu.com/p/54067071



## Prettier

https://segmentfault.com/a/1190000015315545

 // private static host: string = 'http://local.ck.com';







http://graphql.cn/

## react-native-config

全局配置变量,原生皆可使用,在项目中用来控制添加的版本号

https://github.com/luggit/react-native-config



## react-native-DatePicker



https://github.com/xgfe/react-native-datepicker

spinner 模式显示不一致

https://github.com/xgfe/react-native-datepicker/issues/231

手动调用

```
use:
<DatePicker
showIcon={false}
hideText={true}
ref={(ref)=>this.datePickerRef=ref}
...
/>

and from your element:
onPress={() => this.datePickerRef.onPressDate()
```

https://www.jianshu.com/p/6700e0422e6e





## 装饰器

**它的主要作用是给一个已有的方法或类扩展一些新的行为，而不是去直接修改它本身。**

在项目中的使用例子 :

```react
/*
  this.props.navigation.navigate('test',{name:'hengcheng'})
*/
import { withMappedNavigationProps } from 'react-navigation-props-mapper';
@withMappedNavigationProps()
class Test extends Component {
    /*  this.props.navigation.state.params.name  */
    render(){
        return (<View><Text>{this.props.name}</Text></View>)
    }
}
```

还有有个ant-design-pro 里面的表单注解也有使用到,更方便的操作表单,并获取每个表单组的值

https://zhuanlan.zhihu.com/p/30487077

https://aotu.io/notes/2016/10/24/decorator/index.html

https://segmentfault.com/p/1210000008917067/read

## 修改git本地的账号

https://blog.csdn.net/autoliuweijie/article/details/52230165

```
查看
git config user.name
git config user.email
修改
git config --global user.name "username"
git config --global user.email "email"
```

## git

我从master 拉取了一个新的开发分支，我想每天把 master分支上其它人的提交同步到我的开发分支，应该怎么做呢?

```
 git rebase master  // 意思是当前的分支建立在master之上
```

经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令。

```
git stash  //  vscode 有储藏这一选项
git stash list  // 查看储藏列表
git stash apply  // 运用最新的储藏
git stash drop name   // 删除某个储存
```

https://www.jianshu.com/p/4a8f4af4e803

```
关于 rebase 和 merge
关于什么时候使用 rebase，什么时候使用 merge，开发者总结了几条规则：

从 remote 分支拉取更新到本地时，使用 rebase。
当完成 bug 修复或新功能时，使用 merge 将子分支合并到主分支。
没有人应该 rebase 一根共享的分支。
```

https://www.cnblogs.com/Sinte-Beuve/p/9195018.html

https://zhuanlan.zhihu.com/p/34197548

## LayoutAnimation API