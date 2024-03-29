---
title: BIM模型渲染算法及实现
date: 2022-11-28 15:47:49
tags:
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E5%8A%A8%E7%89%A9/%E7%8C%9E%E7%8C%81/67c644aa57667a2087ec3c3729134678222f2e13.jpg%401295w.webp?q-sign-algorithm=sha1&q-ak=AKID80ZfiBYyypEmrDt7HPG37qRUcvvt_rkfrsFUn0LWFUpkLpJUztaa91sXI83lRBCR&q-sign-time=1693270221;1693273821&q-key-time=1693270221;1693273821&q-header-list=host&q-url-param-list=ci-process&q-signature=e181319c83e1061686b68dc52929fd06dcd9349c&x-cos-security-token=A9aMPFvM3Hz6Z4Z1wOck7Kkw3ySNKYRac6177f2d67f02a6f8dbc6d58e7c8942aa-ArzDvussFJBIltuZUF4aZ9dnIxKooQkbCVQbQrjRyePIjhZq3Y739k5Cz1tMbDOFmp42v18yjuVbsyXlk7vFQ8fiW6S-tvX0CT8I4l9_iUURUSndVyK0Z0NuUjpz-wcnbE8Zw8kBHEHaQAZuj1jHacAyczrDFNpP98fQugaXSn2RPvU1s4-mTfYj4LL33d&ci-process=originImage
---

# BIM模型渲染算法及实现

## 一、SSAO算法 屏幕空间环境光遮蔽

### 1. SSAO的介绍

SSAO，即屏幕空间环境光遮蔽(Screen-Space Ambient Occlusion)。要讨论SSAO，首先要了解AO(Ambient Occlusion)，即环境光遮蔽。这是一种着色渲染技术，模拟光线到达物体的能力的粗略的全局方法，描述光线达到物体表面的能力。在一些古老或者简单的游戏中，游戏开发人员通常只给环境光一个单一的颜色值，但这渲染出来的效果远差于真实场景。比如在房间里，角落的光线通常比较暗。这是因为光在反射的时候，很多褶皱、空洞中的光线难以反射出来达到我们的眼睛，所以这些地方看着会更加的暗。

![image-20221128162121381](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/image-20221128162121381.png)

如上图，左边以及中间的图片没有应用AO而显得白色雕像悬浮在空中一样，但其实应像图三一样正确摆放在地面上。

环境光遮蔽采用对空间中每一点发射大量光线，并在片元周围计算光线丢失，这会带来极大的性能开销。

而SSAO通过获取像素的深度缓冲，法线缓冲来计算实现，来近似的表现物体在间接光下产生的阴影。2007年，Crytek公司发布了这个技术，并用在了他们的看家作品孤岛危机上。

### 2. SSAO的原理

对于屏幕空间中每一个片段，我们都根据法线信息来构建一个法向半球，在这个半球里面生成许多随机样本，得到他的深度信息，然后与当前片段深度值进行对比，高于片段深度值的认为被遮蔽，根据这些样本计算加权得到遮蔽因子，这个遮蔽因子会被用来减少片段的环境光照分量。

![image-20221128165033908](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/image-20221128165033908.png)

如上图，图片中灰色的点则是采样点的深度大于该片段的深度，因此当灰色的点越多，则表明当前片段点遮蔽因子越大。

### 3. SSAO算法实现

## 二、CSM算法 级联阴影映射

### 1. CSM算法介绍

阴影贴图(Shadow mapping)是游戏引擎中广泛使用的实现实时阴影的技术，也叫做光照贴图(Light Mapping)，这是一种可以在不减少帧率的情况下达到真实感光照和阴影效果的方法。像素与以纹理形式保存的光照深度缓冲区或者深度图像比较，通过这种方式计算像素是否处于光源照射范围内，从而生成阴影。从光源位置看出去，所有能够看到的物体都处在光照之中，但是这些物体后面的东西将处于阴影之中。光照场景进行渲染，保存能够看到的物体表面深度，即得到深度图像。然后，正常的场景中的每个点都变换到光源的裁剪坐标，然后与这个深度图像进行比较，就好像判断场景中的每个点能否被光线看到，从而进行正常场景的渲染。

![image-20230223092226092](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/image-20230223092226092.png)

![image-20230223092258388](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/image-20230223092258388.png)

基础的阴影贴图方法对于大型场景渲染显得力不从心，很容易出现阴影抖动和锯齿边缘现象。Cascaded Shadow Maps方法根据对象到观察者的距离蹄冻不同分辨率的深度纹理来解决上述问题。他将相机的视锥体分割成若干部分，然后为分割的每一部分生成独立的深度贴图。

CSM通常用来在大型场景模拟太阳投射的阴影。在一张阴影贴图中捕捉所有对象需要阴影贴图具有非常高的分辨率。使用多张阴影贴图就可以结局这个问题。对于进出的场景使用较高分辨率的阴影贴图，对于远处的场景使用粗糙的阴影贴图，在两张阴影贴图过的地方选择其中一张使用。因为远处的对象只占画面的很少一部分像素，而近处的对象占据了画面的很大一部分。

### 2. CSM算法原理

CSM的具体流程如下:

1. **划分camera frustum成多个subfrustum**，其中每个分割面的z值有公式计算得出。[cascaded_shadow_maps (nvidia.cn)](https://developer.download.nvidia.cn/SDK/10.5/opengl/src/cascaded_shadow_maps/doc/cascaded_shadow_maps.pdf)[(PDF) Parallel-split shadow maps for large-scale virtual environments (researchgate.net)](https://www.researchgate.net/publication/220805307_Parallel-split_shadow_maps_for_large-scale_virtual_environments)

![img](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/v2-9dd397c609a3881ccbc70b09e02a5978_r.jpg)

2. **计算每个小的subfrustum的包围盒。** 我们可以发现光锥体要包裹相机视锥体。光锥体就像一个包围盒。但仅仅包裹分割后的相机视锥体是不够的，如果在光源和分割后的相机视锥体之间存在可以投射阴影的遮挡物，我们应该扩展光锥体的大小将遮挡物包裹在光锥体中。此外，通常光锥体包含了很大一部分不可见的区域，造成了阴影贴图的大量浪费。我们可以将分割后的相机视锥体的顶点映射到光源的齐次空间，得到一个与光锥体对齐的包围盒。根据这个包围盒，我们可以使用缩放和平移构造出更加精确的光锥体。我们确实也可以让光锥体和分割后的相机视锥体完全一致，但这可能会改变光源的方向。

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/v2-e3525fcf48b0b35f643426f6560b9436_r.jpg)

3. **重复操作，对每个subfrustum生成一张shadow map。** N一般取1到4之间的整数值。
4. **对每一个像素选择合适的shadow map生成阴影。** 我们需要计算片段对应的阴影贴图的坐标，我们首先使用相机的模型视图矩阵的逆矩阵将片段坐标变换到世界坐标系下，然后将变换后的坐标乘以片段对应的果园矩阵。接着将坐标从[-1,1]线性缩放到[0,1]的范围，即映射到深度贴图上。现在，我们就可以使用这个坐标访问片段对应深度贴图。如果坐标的z值比深度贴图对应的z值小，说明片段被光源照亮，否则，说明片段处于阴影中。

