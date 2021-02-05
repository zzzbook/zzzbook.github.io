---
layout: post
title: Dialog 自定义布局宽高失效解决办法
date: 2021-02-05
categories: Android
tags: Android

---

- 在 Dialog 布局最外层嵌套一层 FrameLayout，并将其宽高设置为 wrap_content，内部布局的宽高即可正常显示。