# Android Support Annotations简介
Android Support Library是Android官方推出的支持扩展库，包含了丰富的组件和工具类。从Android Support Library 19.1版本开始，Android引入了一个新的注解库，它包含很多有用的元注解，可以很好的帮忙开发者管理代码，减少bug，本文就简要介绍一下Android Support Annotations。
## 工程依赖
将以下代码行添加到 build.gradle 文件的 dependencies 块中，向您的项目添加 support-annotations 依赖项：

``` 
compile 'com.android.support:support-annotations:24.2.0' 
```

如果项目中已经依赖了appcompat、support-v4等库，就不需要再显式声明依赖注解库，因为appcompat、support-v4也依赖注解库。

## 空指针注解
@NonNull/@Nullable是support annotations最基础、最有用的注解。@NonNull/@Nullable两个注解可以修饰函数参数或者函数返回值，表示参数或者返回值是否可以为null。

- @NonNull： 表示参数或者返回值不能为null
- @Nullable：表示参数或者返回值可以用null

使用之后Android Studio在代码可能出现不安全的情况下会给出智能提示。

## 资源类注解


- @AnimatorRes ：指出一个integer的参数，成员变量，或方法返回值是一个animator资源的引用。
- @AnimRes：指出一个integer的参数，成员变量，或方法返回值是一个anim资源的引用。
- @AnyRes：指出一个integer的参数，成员变量，或方法返回值是一个任意资源类型的引用。
- @ArrayRes：指出一个integer的参数，成员变量，或方法返回值是一个array资源类型的引用。
- @AttrRes：指出一个integer的参数，成员变量，或方法返回值是一个attr资源的引用。
- @BoolRes：指出一个integer的参数，成员变量，或方法返回值是一个boolean资源的引用。
- @ColorRes：指出一个integer的参数，成员变量，或方法返回值是一个color资源的引用。
- @DimenRes：指出一个integer的参数，成员变量，或方法返回值是一个dimen资源的引用。
- @DrawableRes：指出一个integer的参数，成员变量，或方法返回值是一个drawable资源的引用（包括@mipmap）。
- @FractionRes：指出一个integer的参数，成员变量，或方法返回值是一个fraction资源的引用。
- @IdRes：指出一个integer的参数，成员变量，或方法返回值是一个id资源的引用。
- @IntegerRes：指出一个integer的参数，成员变量，或方法返回值是一个integer资源的引用。
- @InterpolatorRes：指出一个integer的参数，成员变量，或方法返回值是一个interpolator资源的引用。
- @LayoutRes：指出一个integer的参数，成员变量，或方法返回值是一个layout资源的引用。
- @MenuRes：指出一个integer的参数，成员变量，或方法返回值是一个menu资源的引用。
- @PluralsRes：指出一个integer的参数，成员变量，或方法返回值是一个plurals资源的引用。
- @RawRes：指出一个integer的参数，成员变量，或方法返回值是一个raw资源的引用。
- @StringRes：指出一个integer的参数，成员变量，或方法返回值是一个string资源的引用。
- @StyleableRes：指出一个integer的参数，成员变量，或方法返回值是一个styleable资源的引用。
- @StyleRes：指出一个integer的参数，成员变量，或方法返回值是一个style资源的引用。
- @TransitionRes：指出一个integer的参数，成员变量，或方法返回值是一个transition资源的引用。
- XmlRes：指出一个integer的参数，成员变量，或方法返回值是一个xml资源的引用。

## 线程相关注解

- @MainThread：指出被注解的方法应该只在MainThread（主线程）中被调用
- @UiThread：指出被注解的方法应该只在UI线程中被调用
- @WorkerThread：指出被注解的方法应该只在工作线程中被调用
- @BinderThread：指出被注解的方法应该只在binder线程中被调用
- @AnyThread:

构建工具会将 @MainThread 和 @UiThread 注解视为可互换

## 取值范围

- @FloatRange：指出一个被注解的元素应该是一个给定范围内的float值或double值
- @IntRange：指出一个被注解的元素应该是一个给定范围内的int值或long值
- @IntDef：指出一个int类型的元素，它表示的是一个逻辑上的类型，并且它的值必须是被明确声明的常量之一。官方常使用这种方式使int类型代替enum类型
- @StringDef：指出一个String类型的元素，它表示的是一个逻辑上的类型，并且它的值必须是被明确声明的常量之一

## 权限注解
- @RequiresPermission: 指出需要调用者有特定的权限

## 其他注解

- @ColorInt：指出一个被注解的元素，是一个int颜色值，表示的是AARRGGBB
- @Size：表示一个被注解的元素应该有一个给定的大小或长度
- @CallSuper: 指出子类需要调用方法的超类实现
- @VisibleForTesting：可注解一个类，方法，或变量，表示有更宽松的可见性，这样它能够有更宽泛的可见性，使代码可以被测试
- @Keep：确保如果在构建时缩减代码，标注的元素不会移除，一般会添加到通过反射访问的方法和类中，以阻止编译器将代码视为未使用

## 参考文档：

[使用注解改进代码检查](https://developer.android.com/studio/write/annotations.html#thread-annotations)

[Android Support库——support annotations](http://blog.csdn.net/maosidiaoxian/article/details/50452706)

[使用Android Support Annotations优化你的代码](http://www.jianshu.com/p/1d0faca34a6e)

[Android Support Annotations 使用详解](http://www.codeceo.com/article/android-support-annotations-2.html)


