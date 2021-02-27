---
layout: post
title: Android Paint 基础属性
date: 2021-02-27
categories: Android
tags: Android

---

# Paint 基础属性应用

- 方法

  |          方法          |                       描述                        |
  | :--------------------: | :-----------------------------------------------: |
  |         reset          |                     重置画笔                      |
  |        setColor        |                     设置颜色                      |
  |        setAlpha        |                    设置透明度                     |
  |        setARGB         |               设置带透明通道的颜色                |
  |     setStrokeWidth     |      设置描边线条宽度，内外增加各 width / 2       |
  |      getFillPath       | 获取预处理后的 Path，如：增加 Stroke 后可获得描边 |
  |    setSubPixelText     |      设置亚像素，对文本的优化，使文字更清晰       |
  |    setUnderlineText    |                  设置文本下划线                   |
  |    setFakeBoldText     |                   设置文本粗体                    |
  |      setTextAlign      |                   设置文本居中                    |
  |       setShader        |                    设置着色器                     |
  |      setTypeface       |                     设置字体                      |
  |     setShadowLayer     |                   设置阴影效果                    |
  |      setTestSize       |                     设置大小                      |
  |     setTextScaleX      |                     设置缩放                      |
  |      setTextSkewX      |                     设置倾斜                      |
  |    setLetterSpacing    |                    设置行间距                     |
  |      measureText       |                   测量文字宽度                    |
  |       breakText        |                 剪切文字最大长度                  |
  |     getTextBounds      |                   获取文本边界                    |
  |      getTextPath       |                   获取文本路径                    |
  | setFontFeatureSettings |             设置字体样式，可设置 CSS              |
  |     setMaskFilter      |       对图像进行处理，实现绿化，立体等效果        |

- `flags`  标记

  |        标记        |            描述            |
  | :----------------: | :------------------------: |
  |  ANTI_ALIAS_FLAG   |       开启抗锯齿标记       |
  |    DITHER_FLAG     |     启动抖动标记，柔和     |
  | FILTER_BITMAP_FLAG | 缩放的位图上启用双线性采样 |

- `style` 填充方式

  |         方式          |      描述      |
  | :-------------------: | :------------: |
  |      Style.FILL       |    内部填充    |
  | Style.FILL_AND_STROKE | 填充内部和描边 |
  |     Style.STROKE      |      描边      |

- `strokeCap` 线帽样式

  |    样式    |                 描述                 |
  | :--------: | :----------------------------------: |
  |  Cap.BUTT  |                无线猫                |
  | Cap.SQUARE | 与宽度为单位，开头结尾添加半个正方形 |
  | Cap.ROUND  |    以线条为直径，开头结尾添加半圆    |

- `strokeJoin` 线段拐角样式

  |    样式    | 描述 |
  | :--------: | :--: |
  | Join.MITER | 尖角 |
  | Join.BEVEL | 平角 |
  | Join.ROUND | 圆角 |

- `pathEffect` 路径样式

  |     PathEffect     |                          描述                          |
  | :----------------: | :----------------------------------------------------: |
  |  CornerPathEffect  |        圆角效果，将尖角替换为圆角。（圆角矩形）        |
  |   DashPathEffect   |              虚线效果，用于各种虚线效果。              |
  | PathDashPathEffect |      Path 虚线效果，虚线中的间隔使用 Path 代替。       |
  | DiscretePathEffect |                  让路径分段随机偏移。                  |
  |   SumPathEffect    |      两个 PathEffect 效果组合，同时绘制两种效果。      |
  | ComposePathEffect  | 两个 PathEffect 效果叠加，先使用效果1，之后使用效果2。 |

  