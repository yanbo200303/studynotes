## Dexposed
基于Xposed的AOP框架，方法级粒度，可以进行AOP编程、插桩、热补丁、SDK hook等功能。
<p>
Xposed需要Root权限，是因为它要修改其他应用、系统的行为，而对单个应用来说，其实不需要root。 Xposed通过修改Android Dalvik运行时的Zygote进程，并使用Xposed Bridge来hook方法并注入自己的代码，实现非侵入式的runtime修改。比如蜻蜓fm和喜马拉雅做的事情，其实就很适合这种场景，别人反编译市场下载的代码是看不到patch的行为的。小米(onVmCreated里面还未小米做了资源的处理)也重用了dexposed，去做了很多自定义主题的功能，还有沉浸式状态栏等。
<p>
我们知道，应用启动的时候，都会fork zygote进程，装载class和invoke各种初始化方法，Xposed就是在这个过程中，替换了app_process，hook了各种入口级方法（比如handleBindApplication、ServerThread、ActivityThread、ApplicationPackageManager的getResourcesForApplication等），加载XposedBridge.jar提供动态hook基础。
<p>优点：
非侵入式
方法级粒度
<p>缺点：
需要root权限
不支持art
对于代码混淆后，生成patch很困难
## AndFix
同样是方法的hook，AndFix不像Dexposed从Method入手，而是以Field为切入点。
优点：
不需要重启
支持2.3-6.0
支持art和dalvik
轻量
缺点：
方法级别的修复，不能完成资源文件的修改替换
由于是调用了JNI层的方法，那么你懂的某些ROM可能会出问题，兼容性稳定性问题
## ClassLoader
优点：
支持2.3-6.0
支持art和dalvik
稳定性兼容性都很好
缺点：
需要启动后生效 
## 美团Robust
侵入式
## 微信Tinker
Tinker是微信官方的Android热补丁解决方案，它支持动态下发代码、So库以及资源，让应用能够在不需要重新安装的情况下实现更新。

|             | Tinker  | QZone | AndFix  | Dexposed
| 类替换       | yes     | yes   | no      | no
| So替换       | 	yes   | no    | no       | no
| 资源替换    | 	yes   |	yes   |	no 	      | no
| 全平台支持  | yes 	| yes | 	yes | 	no
| 即时生效 	  | no 	| no | 	yes | 	yes
| 性能损耗 	  | 较小 | 	较大 | 	较小 | 	较小
| 补丁包大小   | 较小 | 	较大 | 	一般 | 	一般
| 开发透明 	    | yes 	   | yes | 	no | 	no
| 复杂度       | 较低 	    | 较低 | 	复杂 | 	复杂
| gradle支持  | 	yes      | no | no | 	no
| 接口文档 	  | 丰富        | 较少 	| 一般 | 	较少
| Rom体积 	  | Dalvik较大 	| 较小 	| 较小 | 	较小
| 成功率 	  | 较高 	        | 最高 | 	一般 	| 一般
Dexposed不支持Art模式（5.0+），且写补丁有点困难，需要反射写混淆后的代码，粒度太细，要替换的方法多的话，工作量会比较大。 AndFix支持2.3-6.0，但是不清楚是否有一些机型的坑在里面，毕竟jni层不像java曾一样标准，从实现来说，方法类似Dexposed，都是通过jni来替换方法，但是实现上更简洁直接，应用patch不需要重启。但由于从实现上直接跳过了类初始化，设置为初始化完毕，所以像是静态函数、静态成员、构造函数都会出现问题，复杂点的类Class.forname很可能直接就会挂掉。 ClassLoader方案支持2.3-6.0，会对启动速度略微有影响，只能在下一次应用启动时生效，在空间中已经有了较长时间的线上应用，如果可以接受在下次启动才应用补丁，是很好的选择。
总的来说，在兼容性稳定性上，ClassLoader方案很可靠，如果需要应用不重启就能修复，而且方法足够简单，可以使用AndFix，而Dexposed由于还不能支持art，所以只能暂时放弃，希望开发者们可以改进使它能支持art模式，毕竟xposed的种种能力还是很吸引人的（比如hook别人app的方法拿到解密后的数据，嘿嘿），还有比如无痕埋点啊线上追踪问题之类的，随时可以下掉。

## 参考
热更新？热补丁？热修复？
https://jackl-e-e.github.io/android/2016/05/05/%E7%83%AD%E8%A1%A5%E4%B8%81or%E7%83%AD%E4%BF%AE%E5%A4%8D.html
Android hot fix线上热修复框架比较
http://www.voidcn.com/blog/RichieZhu/article/p-5006460.html
各大热补丁方案分析和比较
http://blog.zhaiyifan.cn/2015/11/20/HotPatchCompare/

[Android热修复] 技术方案的选型与验证
http://www.jianshu.com/p/1683c4e6f36d
