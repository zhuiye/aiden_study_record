# react native module in kotlin

1. build.gradle (Project) 文件添加 如下内容

```js
   ext {

        kotlin_version = '1.3.50'
    }

    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
```

2. build.gradle (app) 文件添加如下内容和

```js
   apply plugin: "com.android.application"
   apply plugin: 'kotlin-android'
   apply plugin: 'kotlin-android-extensions'

   ...
   dependencies {
      implementation 'androidx.core:core-ktx:1.0.2'
      implementation"org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
      implementation 'com.google.android.material:material:1.1.0'
   }

```

3. 编写 SnakeModule.kt

```kotlin
  package com.rnstudy  //包名

  import android.util.Log
  import android.widget.Toast
  import com.facebook.react.bridge.ReactApplicationContext
  import com.facebook.react.bridge.ReactContextBaseJavaModule
  import com.facebook.react.bridge.ReactMethod
  import com.google.android.material.snackbar.Snackbar
  import kotlin.math.log

class SnakeModule(reactContext: ReactApplicationContext):ReactContextBaseJavaModule(reactContext) {

    override fun getName(): String {
        return "SnakeModule" //暴露出去的模块名
    }

    @ReactMethod
    fun show (message: String, duration: Int){
        Log.d("RN","打印结果:"+duration.toString())
        Toast.makeText(reactApplicationContext,message,duration).show()
//        Snackbar.make(,"xxx",Snackbar.LENGTH_LONG).show();
    }

    companion object {
        // 暴露出去的静态变量 如 SnakeModule.SHORT =Toast.LENGTH_SHORT
        private val DURATION_SHORT_KEY = "SHORT"
        private val DURATION_LONG_KEY = "LONG"
    }


    override fun getConstants(): Map<String, Any> {
        // 定义暴露出去的常量
        val constants = HashMap<String, Any>()
        constants.put(DURATION_SHORT_KEY, Toast.LENGTH_SHORT)
        constants.put(DURATION_LONG_KEY, Toast.LENGTH_LONG)
        return constants
    }

}

```

声明包 SnakePage

```kotlin
  package com.rnstudy

  import android.view.View


  import com.facebook.react.ReactPackage
  import com.facebook.react.bridge.NativeModule
  import com.facebook.react.bridge.ReactApplicationContext
  import com.facebook.react.uimanager.ReactShadowNode
  import com.facebook.react.uimanager.ViewManager

  class SnakePackage :ReactPackage {
      // 特别要注意.这里的ViewManager 来自faceBook
      // 这里可以暴露自定义视图模块
      override fun createViewManagers(reactContext: ReactApplicationContext): MutableList<ViewManager<out View, out ReactShadowNode<*>>> {
          return mutableListOf()
      }

    //  添加功能模块
      override fun createNativeModules(
              reactContext: ReactApplicationContext): List<NativeModule> {
          val modules = ArrayList<NativeModule>()

          modules.add(SnakeModule(reactContext))

          return modules
      }
  }
```

最后一步,注册包

```java

  protected List<ReactPackage> getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List<ReactPackage> packages = new PackageList(this).getPackages();
          // Packages that cannot be autolinked yet can be added manually here, for example:
          packages.add(new SnakePackage());
          return packages;
        }
```

## 遇到错误

Attempt to invoke virtual method 'double java.lang.Double.doubleValue()' on a null object reference getDouble

当我调用

```js
NativeModules.SnakeModule.show(
  '哈还',
  NativeModules.SnakeModule.DURATION_SHORT_KEY,
);
```

原因是传入空字符串,暴露出的变量,并不是 DURATION_SHORT_KEY,而是 SHORT, 要知道暴露什么变量,我们可以 console.log(NativeModules.SnakeModule)

### 学习链接

https://proandroiddev.com/react-native-bridge-with-kotlin-b2afde2f70b

https://callstack.com/blog/writing-a-native-module-for-react-native-using-kotlin/

https://juejin.im/post/5e1d7eb06fb9a02ffe7028f1#heading-7
