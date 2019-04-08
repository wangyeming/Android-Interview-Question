View类问题
1. MeasureSpec的理解
2. View的工作流程
3. 如何自定义View
4. View事件分发机制
5. View的滑动冲突处理

# View的绘制流程

## MeasureSpec的理解

MeasureSpec很大程度上决定了一个View的尺寸规格。在测量过程中，系统会将View的layoutParams根据父容器所施加的规则转换成对应的MeasureSpec。对于DecorView而言，其MeasureSpec由窗口的尺寸和其自身的LayoutParams来共同确定。对于普通View而言，则是由父容器的MeasureSpec和其自身的LayoutParams来共同确定的。

MeasureSpec是一个32位的int值，高2位代表SpecMode，低30位代表SpecSize。
SpecMode表示测量模式，SpecSize表示某种测量模式下的规格大小。

SpecMode有三类，分别是UNSPECIFIED,EXACTLY,AT_MOST三种。UNSPECIFIED表示父容器不对View做任何限制，EXACTLY表示父容器已经检测出View的实际精确大小，View最终大小就是SpecSize的大小。AT_MOST则表示父容器制定了可用大小为SpecSize的，子view不能超过这个size。

对于子View而言，其SpecMode规律是，如果自己的布局参数是固定size，那么不管父容器多少，SpecMode都是EXACTLY。如果是match_parent的话，父布局是什么，子布局就是什么。对于wrap_content而言，无论父布局是EXACTLY，还是AT_MOST，子View都是AT_MOST。

## View的工作流程

View的工作流程主要指measure，layout，draw这三大流程。也就是测量，布局，绘制。measure确定View的测量宽高，layout确定View的最终宽/高和四个顶点的位置，而draw则将View绘制到屏幕上。

View的measure过程由measure()方法来完成，而ViewGroup除了完成自己的measure过程外，还会挨个遍历调用所有子元素的measure()方法。
Layout的作用是ViewGroup用来确定子元素的位置。当ViewGroup的位置被确定后，它在onLayout中会遍历所有子元素并调用其layout方法。

View的draw过程遵循一下四步：
1. 绘制背景 drawBackground(canvas)
2. 绘制自己 (onDraw)
3. 绘制children   (dispatchDraw)
4. 绘制装饰 (onDrawScrollBars)

## 如何自定义View

1. 继承View重新onDraw()方法，注意支持wrap_content，处理好padding
2. 继承ViewGroup派生特殊的布局
3. 继承特定的View，比如TextView
4. 继承特定的ViewGroup

# View事件分发

## View事件分发机制

Android的View事件传递从外向内，Activity->Window->View，如果View的onTouchEvent返回false，那么它的父容器的onTouchEvent方法就会被调用。简而言之就是从外向内分发事件，从内向外抛出不处理的事件。

View事件的分发是由三个重要方法来共同完成的，dispatchTouchEvent(),onInterceptEvent(),onTouchEvent()。
对于ViewGroup而言，事件传入进来，首先调用onInterceptEvent判断是否需要拦截，如果拦截，则根据onTouchEvent的返回值判断是否消耗事件，否则，调用子元素的dispatchTouchEvent，根据返回值判断是否消耗事件。

## View的滑动冲突处理

滑动冲突的根本原因在于，内外两层的View都可以滑动，没处理好就会有滑动冲突。滑动冲突可以分为三类，一种是父布局和子元素分别在水平和垂直方向上滑动，典型的如ViewPager配合Frafment时嵌套了ListView，第二种是两个View同时在一个方向上滑动，第三种是两种的混合嵌套。

处理滑动冲突的方式有两种，一种是外部拦截法，一种是内部拦截法。
所谓外部拦截法指的是点击事件都需要先经过父容器的拦截处理，如果父容器需要处理事件就拦截，否则就不拦截。复写父容器的onInterceptEvent方法，不需要的事件就不要拦截。
所谓的内部拦截法指的是事件如果不被子元素消耗，会被抛出到父容器。改写子元素的dispatchTouchEvent方法，对于不需要的事件，调用父布局的requestDisallowInterceptTouchEvent()即可。

