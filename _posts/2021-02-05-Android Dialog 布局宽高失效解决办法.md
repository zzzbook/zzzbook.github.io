---
layout: post
title: Dialog 布局问题
date: 2021-02-05
categories: Android
tags: Android

---

## 布局宽高设置失效问题

- 在 Dialog 布局最外层嵌套一层 FrameLayout，并将其宽高设置为 wrap_content，内部布局的宽高即可正常显示。

## DialogFragment 全屏显示问题

- 在 `onCreateDialog` 中设置系统布局 `padding` 为 0，且修改 `window` 的 `layout` 为 `MATCH_PARENT`

  ```kotlin
  window.decorView.setPadding(0, 0, 0, 0)
  window.setLayout(WindowManager.LayoutParams.MATCH_PARENT, WindowManager.LayoutParams.MATCH_PARENT)
  ```

  

