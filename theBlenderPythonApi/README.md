# The Blender Python API: Precision 3D Modeling and Add-on Development

![](https://chrisconlan.com/wp-content/uploads/2017/06/blender_python_api-717x1024.png)

by[Chris Conlan](https://github.com/chrisconlan)


# Introduction

本文详细介绍了Blender Python API中3D建模工具的开发和使用。我们挑战通过构建精确的数据驱动模型，将Blender视为纯粹的艺术家工具。
同时，我们通过在熟悉的Blender环境中部署自定义工具，教你如何帮助和启用艺术家。本文中提供的知识是对Blender’s的深刻理解的结果，文档和
源代码，以及Blender核心编写的附加组件的源代码开发人员。作者发现了许多又用的功能，截至撰写本文时，无证。值得庆幸的是，我们用户可以通过
倾听和学习来保持最前沿开发人员。本文统一了文档齐全的介绍性材料和未记录的高级材料创建一个强大的参考。本书包含代码示例和强大脚本和附加组件的屏幕截图。我们包括用于自动执行精确任务的脚本，否则这些任务很难手动实现。此外，我们使用新工具，对象和自定义来构建附加组件，以增强Blender现有的功能选项。

# Definitions

3D建模是操纵数据以创建对象和环境的3D表示的艺术。3D艺术家使用以下工具和技术来构建3D模型。

  1 手动建模涉及艺术家与软件界面交互，这可以是
  
    1 使用3D建模套件（blender，Mayahuo3d Max）进行创建和编辑手工制作的物品
    2 使用3D建筑元素（Minecraft，Fallout4或Sims）玩视频游戏
    4 手动将数据输入到3D对象文件（.obj,.stl或glTF）
 
  2 自动建模涉及算法生成3D模型。这可以是：

    1 程序生成视频游戏中的环境和角色
    2 根据建筑规范生成详细的建筑模型
    3 从分形算法生成3D打印艺术
    
  3  基元是3D模型的基本构建块。虽然没有严格的关于什么构成原语的规则，这些可以是：

    1 简单的封闭形状，如平面，立方体和金字塔
    2 简单的曲线形状，如球形，圆柱形和锥形
    3 复杂的形状，如tori（复数圆环），Bezier曲线，Nurbs曲面
    
3D模型是对象和环境的数据表示。3D模型具有以下特征组件

  1 数据格式允许模型通过应用程序进行区分和专门化。每种类型的3D模型都具有指定它的格式。这些包括：

    1 特定于套件的格式，例如.blender for Blender,.3ds for 3ds Max 和 .ma for Maya
    2 特定于渲染器的格式，如.babylon for BabylonJS,.json几何描述符（3JS）和.glsl(OpenGL着色器）
    3 简单的交换格式，如.obj和.stl
    
  2 顶点和面定义了在3D空间中连接这些点的点和曲面。
  
    1 顶点是实数3D空间的三元组，或对象的每个点的传统（x,y,z)坐标
    2 面是整数的三元组，其中（i，j，k）表示由第i，第j和第k个顶点形成的3D空间中的三角形。
    
# 本书的必备知识

本书涵盖了运行Python3.5.2的Blender版本2.78c。大多数例子都在Blender2.70和更大，这些概念一般适用于Blender。
尽管如此，建议读者使用Blender2.78c最好跟随。在我们讨论Blender和Python语言的历史和发展时，
我们将指出不太可能用于过去和未来版本的编程实践。

  我们假设Blender和Python3的基本工作知识。熟悉任何版本的Blender2.60或更高版本就足够了。同样，纯Python2程序员也没有问题。
  
# 资料概述

本文介绍知识并依据它构建，以创建越来越完整和复杂的软件解决方案。我们介绍和讨论以下主要议题

## [第一章 Blender 接口](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter1.md)

Blender有许多独立的接口。核心接口具有容易交互因为几乎所有可能的用户都直接与Python函数相关联。
我们对Python编程特别重要的部分建立了相似性。

Blender接口将充当你的软件的部署和开发环境。我们讨论了在Blender接口中保留编程和测试Python的特别注意事项。

为了尽量减少整篇文章中截图的使用，我们引入了重要的词汇来讨论Blender接口。使用这个词汇表，我们可以专注于Python代码，
同时允许用户在他们自己喜欢的Blender接口的首选布局中工作。

## [第二章 bpy模块](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter2.md)

bpy模块是Blender Python API的核心。学习浏览此模块将大大提高你对Blender和API的理解。在本书的早期，
我们专注于构造对象和操纵其关联元数据bpy中的类。在本书的后面，我们访问bpy模块中的新类，将脚本转换为插件。

模块本身非常冗长。早期的脚本既复杂又重复。在完成对象创建和操作后，我们将开始为我们将在本书中构建的工具包添加有用的函数。
我们将在工具包中存储复杂且常用的算法，但鼓励读者将bpy模块的核心元素提交到内存中。通过这种方式，我们创建了易于编写且易于共享的代码。

## [第三章 bmesh模块](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter3.md)

bmesh模块是一个相对较新的模块，它试图简化对象数据的复杂顶点级操作。对于那些熟悉Blender的读者来说，
bmesh中的大多数操作只能在编辑模式而不是对象模式下运行。这有助于强制bmesh中的函数用于粒度变化而不是网格数据的全局变换。

作者认为，该模块将Blender Python API与其他自动3D建模软件区分开来。bmesh模块为我们提供了对Blender的大型编辑模式工具的算法访问，
用于顶点级，边级和面级对象操作。它允许我们为数百而不是数千代码的非常复杂的对象编写程序生成算法。

## [第四章 建模和渲染主题](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter4.md)

对于从事3D建模工作的人来说，必须对我们依赖的机制有一个基本的了解，以便呈现和和可视化我们的工作产品。
我们将讨论渲染管道的基础知识以及Blender Python开发的重要渲染主题。
Blender和我们导出的可视化器中的许多感知错误和奇怪行为实际上是渲染器的预期行为。
我们学习如何检测和编程这些行为，以确保我们创建高度可移植的模型。

我们讨论常见和不常见的文件格式，Z-fighting，法向量，软件和硬件渲染之间的差异等等。
这将帮助我们根据我们在各种渲染软件中看到的行为来调试Python代码。

## [第五章 插件开发简介](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter5.md)

弥和脚本和可分发插件之间的差距可能是一个困难的过程，它依赖于非常具体的开发实践、谨慎的代码组织和偶尔的元编程。
其中许多概念反映了标准的Python模块开发实践，而其他许多概念则依赖于Blender脚本接口的独特行为。

我们在这里详细讨论GUI开发，自定义Blender数据对象，bpy.type和bpy.utils。
我们讨论了插件的组织以及增加不同版本Blender的可移植性的方法。在本文的这一点上，读者将能够创建扩展Blender的插件，
以获得具有Python经验的建模者的好处。

## [第六章 bgl和blf模块](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter6.md)

bgl模块是Blender的OpenGL包装器，通过Blender接口可用于标记、测量和可视化对象和数据。blf模块用于使用Blender接口绘制文本和字体，
很少在没有bgl模块的情况下使用。这里我们使用bpy_extrs和mathutils模块来帮助我们

这些模块对于插件开发非常有用，因为我们可以影响用户看到的数据而不会影响模型本身。我们在本文的这一点上介绍它们，
因为它们的有效性取决于将它们作为插件运行的能力。

## [第七章 高级插件开发](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter7.md)

到目前为止，我们将使用Blender的文本编辑器来创建脚本和插件。文本编辑器介绍了我们在此处克服的插件形式的各种限制。
我们还引用了流行的社区插件，讨论了数据存储和模块管理的最佳实践。我们在本章结束时讨论了高级GUI开发。

## [第八章 纹理和渲染](https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter8.md)

到目前为止，我们将完全使用Blender中的网格。在本章中，我们通过纹理和渲染将场景变为现实。我们讨论程序uv映射，
光照布置和相机定位。通过此讨论，概述了照明类型，摄像机视角动态和边界框算法。

我们通过程序化渲染任意场景并为自动渲染管道提供框架来结束本章。我们专注于本章中的静态渲染，
但对自动化动画感兴趣的读者将能够毫不费力地扩展示例。

# Blender和Python的历史

Blender接口和Blender Python API之间的关系在软件开发领域是罕见的。支持API的平台通常将用户和开发人员视为单独公民类别，
包括单独的工具，单独的环境和单独的目标。另一方面，Blender已经消除了开发人员和用户之间的界限，使用户可以轻松地充当开发人员，反之亦然。

开发人员和用户之间的密切关系是Blender核心开发团队中明智的早期设计决策的产物。在Blender于2003年8月作为免费开源软件发布为版本2.26之前，
核心开发团队发布了当时高级版本2.25的Python API文档。Pyhon2.0刚刚于2000年10月发布，Blender已经使用它来管理从接口到其C级数据结构的调用。

Blender2.50和forward将于2009年发布，它将使用纯Python将编辑任务给其较低级别的算法和数据结构。用户界面上的每个操作都链接到Python函数，
用户可以选择从控制台和脚本访问和调用这些函数。

随着我们在2010年初开始，Blender艺术家将越来越意识到Python脚本对建模体验的影响。某些插件将成为对某些领域感兴趣的艺术家的必备品。
其他3D建模软件的开发人员正在开发输出端口，将Blender移植到他们的软件中。今天，Blender有其模块化，以感谢其庞大的人才库，
高薪的职业就业机会和积极的发展社区。


[link](http://file.allitebooks.com/20170616/The%20Blender%20Python%20API.pdf)


<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/README.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter1.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
