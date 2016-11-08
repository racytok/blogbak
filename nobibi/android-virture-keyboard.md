---
title: Android 获取虚拟按键高度
date: 2016-11-02 11:28:31
tags: Android
categories: Android技术文章
---
在android开发中，我们往往对于虚拟键盘问题比较头疼，其实我们可以通过屏幕尺寸的差值来判断是否有虚拟键盘，从而分情况处理。

### 1.不带虚拟键盘的手机，我们是这样获取屏幕高度的。

```java
  /**
     * 获取屏幕尺寸，但是不包括虚拟功能高度
     *
     * @return
     */
    public int getNoHasVirtualKey() {
        int height = getWindowManager().getDefaultDisplay().getHeight();
        return height;
    }
```
### 2.带有虚拟键盘的情况，我们通过反射获取高度。

```java
 /**
     * 通过反射，获取包含虚拟键的整体屏幕高度
     *
     * @return
     */
    private int getHasVirtualKey() {
        int dpi = 0;
        Display display = getWindowManager().getDefaultDisplay();
        DisplayMetrics dm = new DisplayMetrics();
        @SuppressWarnings("rawtypes")
        Class c;
        try {
            c = Class.forName("android.view.Display");
            @SuppressWarnings("unchecked")
            Method method = c.getMethod("getRealMetrics", DisplayMetrics.class);
            method.invoke(display, dm);
            dpi = dm.heightPixels;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return dpi;
    }
```
> 我们在代码编写时，真正的内容高度其实是屏幕高度减去虚拟键盘的高度，所以虚拟按键的高度： getHasVirtualKey() - getNoHasVirtualKey()

我们写个方法来测试下：
```java
 /**
     * 获取虚拟键的高度
     */
    private void getVirtuakeyHight() {
        Logger.d("不包含虚拟键的高度=" + getNoHasVirtualKey());
        Logger.d("包含虚拟键的高度=" + getHasVirtualKey());
        Logger.d("虚拟键的高度=" + (getHasVirtualKey() - getNoHasVirtualKey()));
    }
```
测试结果：
```java
不包含虚拟键的高度=1776
包含虚拟键的高度=1920
虚拟键的高度=144
```
