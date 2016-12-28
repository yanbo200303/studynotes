随着移动互联网的飞速发展，Android应用安装包的size也随之极速膨胀，目前Android应用的大小普遍为几十M甚至上百M，很影响用户体验。本文对常用的Android应用安装包的size优化的方法做一个小结。
## Apk的结构
在对Apk进行优化之前，需要先了解Apk文件的构成，以下为Apk的文件结构：<p>
--res<p>
--META-INF<p>
--lib<p>
--assets<p>
--resources.arsc<p>
--classes.dex<p>
--AndroidManifest.xml<p>
其中，res文件夹下是资源文件，assets一般存放的也是资源文件，resources.arsc包含了预编译之后的资源文件；lib为使用的第三方库文件；classes.dex为java代码的编译文件；AdnrodManifest.xml为App应用的配置文件；META-INF存储的是关于签名的一些信息。

## 资源文件
毫无疑问，资源文件一般情况下是应用中size最大的部分，所以对App进行大小优化，要首先考虑资源文件的优化。以下是一些资源优化方法：
* 删除无用资源，可使用Lint工具帮助查找，去除没有使用到的图片、String、XML等
* 检查assets目录，确保没有无用的资源文件
* 有选择性的使用hdpi，xhdpi，xxhdpi的图片资源，优先提供xhdpi的图片，对于mdpi，ldpi与xxxhdpi根据需要提供有差异的部分即可
* 图片格式，优选使用webp格式，在某些时候jpeg比png小
* 如果有条件多使用9-patches格式，同时可拉伸区域尽量切小
* 能用代码绘制实现的功能，尽量不要使用大量的图片
* 尽可能的重用已有的图片资源，例如对称的图片，只需要提供一张，另外一张图片可以通过代码旋转的方式实现
* 使用微信的开源资源混淆工具AndResGuard进行资源混淆和压缩

## 第三方库
在App开发过程中，一般都会用到很多第三方库文件，在某些情况下第三方库文件甚至比资源文件还要大，也是重点考虑的优化对象。以下是一些优化方法：
* 移除无用的依赖库
*  限制app支持的cpu架构的数目，在当前的Android生态系统中，一般只需支持armabi和x86架构。如果非必须，可考虑去掉x86的部分
* 

## 代码层面
相对来说，代码在整个App中的size比重没有那么高，不过依然需要我们用一些方法进行优化：
* 去掉各种死代码和无用代码
* 掌握良好的编码习惯，简化代码逻辑
* 使用Proguard，在代码编译时对代码进行混淆，优化和压缩

## 参考资料
[Android如何缩减APK包大小](http://www.cnblogs.com/liuyu0529/p/5759200.html)

[如何给你的Android 安装文件（APK）瘦身](http://www.2cto.com/kf/201411/353176.html)

[微信Android资源混淆打包工具，如何让应用安装包立减1M](https://my.oschina.net/bugly/blog/536064)

[如何优化Android/iOS应用安装包大小](https://www.zhihu.com/question/38836722/answer/78371681)
