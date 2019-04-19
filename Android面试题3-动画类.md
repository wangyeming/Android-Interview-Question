1. Android中动画的种类
2. 来源库Lottie原理

# Android中动画的种类

Android中动画的种类有View动画，帧动画和属性动画。

**View动画** View动画的作用对象是View，包括四种动画效果，平移，旋转，缩放和透明度动画。分别对应着TranslateAnimation, RotateAnimation, ScaleAnimation, AlphaAnimation。如果自定义View动画，可以继承Animation类，实现initialize和applyTransform方法即可。

**帧动画** 帧动画是按顺序播放一组定义好的图片，可以通过xml定义一个AnimationDrawable来实现，注意图片大小，防止OOM。

**属性动画** 属性动画可以对任意对象进行动画而不只是View，效果是在一个时间段内完成对象一个属性值到另一个属性值的改变。其中有一些常用类 ValueAnimator,ObjectAnimator和AnimatorSet等。另外插值器TimeInterpolator和估值器TypeEvaluator，前者的作用是根据时间的流逝来计算属性值改变的百分比，而后者的作用是根据属性改变的百分比计算出改变后的属性值。

此外17年的时候，谷歌推出了物理动画的support库,DynamicAnimation,其中重要的动画有弹簧动画Spring Animation和抛掷动画Fling Animation。

# 来源库Lottie原理

lottie由Airbnb推出的移动端动画实现框架。Lottie使用json文件来作为动画数据源, 这个文件作用是把图片中的元素进行来拆分，并且描述每个元素的动画执行路径和执行时间。Lottie的功能就是读取这些数据，然后绘制到屏幕上。

Lottie 提供了一个 LottieAnimationView 给用户使用，而实际 Lottie 的核心是 LottieDrawable，它承载了所有的绘制工作，LottieAnimationView则是对LottieDrawable 的封装，再附加了一些例如 解析 的功能。