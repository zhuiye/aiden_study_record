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

## js 端传入一个数组到原生端的转换

```kotlin
   @ReactMethod
    fun simpleDialog(title:String,items:ReadableArray,theme:Int,callback: Callback){
       val stringArr=items.toArrayList().toArray()  // 这里需要转换为java 中的 数组类型
       val size=stringArr.size                     // 再进行kotlin 的转化
       val innerItems= arrayOfNulls<String>(size)

       for(index in 0 until size){
           innerItems[index]=stringArr[index].toString()
       }

        // 转化得到  innerItems->string []

        MaterialAlertDialogBuilder(currentActivity,theme)
                .setTitle(title)
                .setItems(innerItems) { dialog, which ->
                    callback.invoke(which)
                    // Respond to item chosen
                }
                .show()
    }
```

## 遇到错误

Attempt to invoke virtual method 'double java.lang.Double.doubleValue()' on a null object reference getDouble

当我调用

```js
NativeModules.SnakeModule.show(
  "哈还",
  NativeModules.SnakeModule.DURATION_SHORT_KEY
);
```

原因是传入空字符串,暴露出的变量,并不是 DURATION_SHORT_KEY,而是 SHORT, 要知道暴露什么变量,我们可以 console.log(NativeModules.SnakeModule)

还有就是报 Didn't find class "androidx.widget.AppCompatTextView" 没有找到,其实就是 xml 那里引入包错误了,在 androidx.widget 下面没有 AppCompatTextView,通过 android studio 就可以 排除错误了

### 学习链接

https://proandroiddev.com/react-native-bridge-with-kotlin-b2afde2f70b

https://callstack.com/blog/writing-a-native-module-for-react-native-using-kotlin/

https://juejin.im/post/5e1d7eb06fb9a02ffe7028f1#heading-7

https://juejin.im/post/5dc14bfc6fb9a04aac11355e

### androidX 迁移

https://www.jianshu.com/p/4844cd2568c5

### android 储存知识

https://juejin.im/post/5de7772af265da3398561133
