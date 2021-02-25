---
layout: post
title: Android ViewGroup / View 自定义流程
date: 2021-02-23
categories: Android
tags: Android

---

# ViewGroup / View 自定义流程

## 基础概念

- 在自定义控件之前，我们先简单了解一下关于宽度与高度的范围定义，高度同理。以上范围定义会分别在测量过程与定位过程中使用到。
  - 控件的实际宽度 `realWidth = paddingLeft + contentWidth + paddingRight`
  - 控件的占位宽度 `occupyWidtd = marginLeft + viewRealWidth + marginRight`

## 测量（onMeasure）

无论是 View 还是 ViewGroup 都需要测量自身的宽高，View 是直接通过 MeasureSpec 参数进行测量，而 ViewGroup 是将 MeasureSpec 参数传递给子元素的测量方法获取宽高后，通过定义的排列方式计算出宽高进行测量。

### MeasureSpec 

MeasureSpec 是一个 32 位的 int 值，高 2 位代表 SpecMode，即测量模式；低 30 位代表 SpecSize，即该模式下测量大小；值得注意的是，此时的测量大小并非 View 最后的实际宽高，定位过程决定了 View 的实际宽高。MeasureSpec 的数值通常由其父容器的 MeasureSpec 与自身的 LayoutParams 决定。

- 测量模式
  1. UNSPECIFIED：待定，表示父 View 对子 View 大小不做限制
  2. EXACTLY：精确，父 View 计算好子 View 具体宽高，子 View 最终大小就是 SpecSize 的值
  3. AT_MOST：最大，父 View 指定一个最大值，子 View 不可超出这个大小

- 测量结果

| Group 测量模式 \ LayoutParams 参数 | 精确值          | MATCH_PARENT            | WRAP_CONTENT            |
| ---------------------------------- | --------------- | ----------------------- | ----------------------- |
| EXACTLY                            | 精确值，EXACTLY | Group Size，EXACTLY     | Group Size，AT_MOST     |
| AT_MOST                            | 精确值，EXACTLY | Group Size，AT_MOST     | Group Size，AT_MOST     |
| UNSPECIFIED                        | 精确值，EXACTLY | Group Size，UNSPECIFIED | Group Size，UNSPECIFIED |

### 测量过程

1. 自定义 View 需要重写 `onMeasure()`方法，否则 `WRAP_CONTENT` 相当于 `MATCH_PARENT`，ViewGroup 可根据自己的排列需求重写该方法
2. 通过 `MeasureSpec.getMode()` 获取测量模式，`MeasureSpec.getSize()` 获取测量大小，View 根据测量模式计算自身的大小，ViewGroup 需要调用 `measureChild()` 方法后，获取子元素大小根据定义的排列方式计算出宽高。
3. 测量完成需要调用 `setMeasuredDimension()` 方法来设置数据，否则会抛出异常。`getMeasureWidth()` 与 `getMeasureHeight()` 所获取的则为测量的值。

## 定位（onLayout）

定位主要是 ViewGroup 执行的过程，对于子元素的位置进行确定。值得注意的是 `onLayout()` 是 ViewGroup 测量子元素的重写方式，`layout()` 是定位自身的方法，不要混淆。

### 定位过程

1. 重写 `onLayout()` 方法
2. 通过 `measureChild()` 方法测量子元素的宽高，然后使用 `getMeasureWidth()` 获取测量宽度，`getMeasureHeight()` 获取测量高度
3. 最后通过 `child.layout()` 方法确定子元素位置。`getWidth()` 与 `getHeight()` 所获得则为定位的值。

注：`getWidth()` 数值是在 Layout 过程测量，`getMeasureWidth()` 数值是在 Measure 过程测量。本质上两个数值保持一致，但是当重写 Layout 方法，重新定位并且扩展控件大小时则导致不一致。 

## 绘制（onDraw）

绘制过程则是真正的将图像绘制在屏幕上的过程。

### 绘制过程

1. `drawBackground()` 绘制背景
2. 保存 Canvas 图层为后续淡出做准备（可选）
3. `onDraw()` 绘制 View 的内容
4. `dispatchDraw()` 绘制子 View
5. 绘制淡出边缘并恢复 Canvas 图层（可选）
6. `onDrawForeground()` 绘制装饰

## 自定义元素

### 自定义属性

- 定义方式：attrs.xml

  ```xml
  <declare-styleable name="">
      <attr name="" format="integer/float/boolean" />	<!-- boolean -->
      <attr name="" format="string" />				<!-- 字符串 -->
      <attr name="" format="color" />					<!-- 颜色 -->
      <attr name="" format="dimension" />				<!-- 单位 -->
      <attr name="" format="reference" />				<!-- 资源 -->
      <attr name="" format="flag" />					<!-- 位运算 -->
      <attr name="" format="fraction" />				<!-- 百分数 -->
      <attr name="" format="enum">					<!-- 枚举 -->
          <enum name="" value="" />
      </attr>
  </declare-styleable>
  ```

- 获取方法

  ```kotlin
  val typeArr = context.theme.obtainStyledAttributes(attrs, R.styleable.xxx, 0, 0)
  try {
      variable = typeArr.getxxx(R.styleable.xxx_xxx, default)
  } finally {
      typeArr.recycle()
  }
  ```

### 优化布局

- 当需要载入布局时，布局可使用 `<merge>` 标签，减少嵌套布局进而优化，可以使用 `LayoutInflater.from().inflate()` 渲染，参数 `attachToRoot: true`

  - XML 布局方式

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <merge xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:parentTag="android.widget.RelativeLayout">
        <!-- 载入布局后 merge 将被省略，仅保留内部内容 -->
        <TextView
            android:id="@+id/trackView"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"/>
    </merge>
    ```

  - 渲染方式

    ```kotlin
    View.inflate(context, R.layout.xxx, this)
    // 或
  LayoutInflater.from(context).inflate(R.layout.xxx, this, true)
    ```
    

### 动态添加元素

- 当需要动态添加布局时，需要重写 `generateLayoutParams()` 方法，以返回默认 LayoutParams，通常继承 `ViewGroup.MarginLayoutParams`

  ```kotlin
  override fun generateLayoutParams(attrs: AttributeSet): LayoutParams {
      return MarginLayoutParams(context, attrs)
  }
  
  override fun generateDefaultLayoutParams(): LayoutParams {
      return MarginLayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT)
  }
  
  override fun generateLayoutParams(p: LayoutParams): LayoutParams {
      return MarginLayoutParams(p)
  }
  ```

### 方法时机

- 当布局内子元素被映射为 XML，全部加载完成则触发 `onFinishInflate()` 可使用 `getChildAt()` 等对于元素进行操作，`onMeasure()` 操作在此之后
- 当控件大小发生改变时调用 `onSizeChanged()` ，可以获取控件的大小。执行顺序 `layout -> setFrame -> onSizeChanged -> onLayout`
- 当控件可见且进行交互时，`onWindowFocusChanged()` 被调用，在该方法内可以获取控件真实宽高

### 刷新视图

- `invalidate()` 请求重绘 View 树，即重新执行 `draw()` 方法；
- `requestLayout()` 调整布局，会重新执行 `measure()` 与 `layout()` 方法，但不会执行 `draw()`;





