---
title: android解决ScrollView嵌套mapView冲突问题
date: 2016-11-04 16:37:37
tags: Android
categories: Android技术文章
---
## 背景

　　在android开发中，有这样一个页面：上面部分是商品的信息介绍，下面用地图显示商家位置信息（信息超过一屏）。我们往往会选择在整个布局的外层套一个ScrollView。当我们滑动地图的时候，就会和ScrollView的滑动事件产生冲突。
　　
## 解决方案：
以下是高德提供的解决方案。

```java
aMap = mapView.getMap();
aMap.setOnMapTouchListener(new AMap.OnMapTouchListener() {
          @Override
          public void onTouch(MotionEvent motionEvent) {
              int action = motionEvent.getAction();
              switch (action) {
                  case MotionEvent.ACTION_UP:
                      mScrollView.requestDisallowInterceptTouchEvent(false);
                      break;
                  case MotionEvent.ACTION_DOWN:
                  case MotionEvent.ACTION_MOVE:
                      mScrollView.requestDisallowInterceptTouchEvent(true);
                      break;
              }
          }
      });
```