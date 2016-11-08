---
title: Android填坑之输入框被挡问题
date: 2016-11-01 13:38:05
tags: Android
categories: Android技术文章
---
## 引言

开发做得久了，总免不了会遇到各种坑。下面介绍两种方案来填这个坑。

## 两种方案

### 1.普通Activity（不带WebView），直接使用adjustpan或者adjustResize

```java
<activity
    android:name=".MainActivity"
    android:windowSoftInputMode="adjustPan"  >
    ...
</activity>
```
一般来说，他们都可以解决问题，当然，adjustPan跟adjustResize的效果略有区别。

adjustPan是把整个界面向上平移，使输入框露出，不会改变界面的布局；
adjustResize则是重新计算弹出软键盘之后的界面大小，相当于是用更少的界面区域去显示内容，输入框一般自然也就在内了。

### 2.如果带WebView：

> * 如果非全屏模式，可以使用adjustResize
> * 如果是全屏模式，则使用AndroidBug5497Workaround进行处理。

#### 使用方式

> * 把AndroidBug5497Workaround类复制到项目中
> * 在需要填坑的activity的onCreate方法中添加一句AndroidBug5497Workaround.assistActivity(this)即可。

#### 关于AndroidBug5497Workaround

这个炫酷AndroidBug5497Workaround类，其实并不是很复杂，只有几十行代码，先贴在这里：

```java
public class AndroidBug5497Workaround {

    // For more information, see https://code.google.com/p/android/issues/detail?id=5497
    // To use this class, simply invoke assistActivity() on an Activity that already has its content view set.

    public static void assistActivity (Activity activity) {
        new AndroidBug5497Workaround(activity);
    }

    private View mChildOfContent;
    private int usableHeightPrevious;
    private FrameLayout.LayoutParams frameLayoutParams;

    private AndroidBug5497Workaround(Activity activity) {
        FrameLayout content = (FrameLayout) activity.findViewById(android.R.id.content);
        mChildOfContent = content.getChildAt(0);
        mChildOfContent.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            public void onGlobalLayout() {
                possiblyResizeChildOfContent();
            }
        });
        frameLayoutParams = (FrameLayout.LayoutParams) mChildOfContent.getLayoutParams();
    }

    private void possiblyResizeChildOfContent() {
        int usableHeightNow = computeUsableHeight();
        if (usableHeightNow != usableHeightPrevious) {
            int usableHeightSansKeyboard = mChildOfContent.getRootView().getHeight();
            int heightDifference = usableHeightSansKeyboard - usableHeightNow;
            if (heightDifference > (usableHeightSansKeyboard/4)) {
                // keyboard probably just became visible
                frameLayoutParams.height = usableHeightSansKeyboard - heightDifference;
            } else {
                // keyboard probably just became hidden
                frameLayoutParams.height = usableHeightSansKeyboard;
            }
            mChildOfContent.requestLayout();
            usableHeightPrevious = usableHeightNow;
        }
    }

    private int computeUsableHeight() {
        Rect r = new Rect();
        mChildOfContent.getWindowVisibleDisplayFrame(r);
        return (r.bottom - r.top);// 全屏模式下： return r.bottom
    }

}
```
#### 最后附上github上AndroidBug5497Workaround地址

[https://github.com/racytok/AndroidBug5497Workaround](https://github.com/racytok/AndroidBug5497Workaround)
