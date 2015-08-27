# ASPI(Android 服务提供接口)

## 框架的预期
框架抽象出android广泛使用到的独立API，并提供默认实现，且支持框架使用者替换某个/某些API的实现。

## 为什么要用SPI
实现上述功能，可以通过很多方式，比如：

- 设置抽象工厂类，使用者继承并实现该工厂，通过setter方法设置到框架中；
- 或者不要工厂类，直接让使用者提供API的实现类；

但无论是哪种，只要是使用**“抽象-继承”**的方式替换API，都需要改动到具体设置的代码块。

无论是对框架扩展本身，还是使用者，更长远地看，还是选择SPI的方式。

对于框架本身：

- 兼容性、包容性更强，让自己成为一个孵化池；
- 一旦有更好的默认实现，更容易地更新替换；

对于使用者：

- 替换API实现时，不要改动APP代码（如果每个API都封装成library模块的话），起码不需要每次重新进行设置；
- 灵活性更强，想怎么组织API实现结构就怎么组织，可谓实现“五花八门”，使用者“为所欲为”；




> 一般地，推荐将不同API的实现单独拎出来建立成library模块，这样替换性更强，牵一发只动一发；
> 
> 但这一点不会限定死~


## Android SPI

### issue: Using serviceloader on android
在Android中使用ServiceLoader跟java中稍有区别，不是将`META-INF/services/*`放在java src中；

如若放在java src中，由于ApkBuilder会将`META-INF/services/*`排除在外（见[issue: ApkBuilder prevent to use ServiceLoader by exluding META-INF folder during build](https://code.google.com/p/android/issues/detail?id=59658)）；

同时，ProGuard也会横插一杠，对无用的文件进行删除或者重命名（具体见[issue: Using serviceloader on android](http://stackoverflow.com/questions/5760607/using-serviceloader-on-android)）；

但还是有办法的，如：

- right click on <project_name>/src/main (where you have /java and /res folders), 
- select New > Folder > Java Resources Folder,
- click Finish (do not change Folder Location), 
- right click on new /resources folder,
- select New > Directory
- enter "META-INF" (without quotes),
- right click on /resources/META-INF folder,
- select New > Directory
- enter "services" (without quotes)
- copy any file you need into /resources/META-INF/services

参见 [issue: create java resources](https://code.google.com/p/android/issues/detail?id=59658#c22)（该文件夹中的文件最终会放在apk压缩包中的根目录下）；

下面也有反馈说，不同库中使用了相同文件名的配置文件，叠加时会报错，但我测试过没有复现，而是“谁最后编译用谁”的方式覆盖了，是否新版本studio或者android tools等工具已经更改？

<br/>

当然，还有其他的方式，比如重写ServiceLoader，将配置保存在assets中等等。

<br/>
---
### issue: Using serviceloader on android:

> The **META-INF** folder is deliberately excluded from the APK by ApkBuilder; 

> the only comment in ApkBuilder.java is **"we need to exclude some other folder (like /META-INF)"**, but there is no other explanation.

> Even after adding META-INF with ant, you will still get in trouble if you want to use Proguard, which refuses to replace the content of META-INF/services/* files or rename them