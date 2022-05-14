---
layout: post
title: "[Nand to Tetris] 从0到1设计一台自己的计算机，最有趣的计算机系统课！"
date: 2022-05-14 14:00:00 +0800
categories: [Course Notes, Nand2Tetris]
tags: [computer system, operating system, compiler, logic gates, Boolean algebra, 计算机系统]
---

相信非计算机软件科班出身（我自己），但仍然因为热爱而从事计算机软件开发尤其是偏底层的嵌入式软件开发的同学们，在工作中随着技术领域的深入，或多或少都能体会到如果没有一个完整的计算机知识体系作为支撑， 很多技术细节总是有一种知其然但不知其所以然的感觉。


<figure>
<img src="/assets/img/nand2tetris/stackoverflow_keyboard.png" alt="stack overflow keyboard" class="center" width="600"/>
<figcaption>Fig.1 - Imagine a world without StackOverflow <a href="https://stackoverflow.blog/2021/03/31/the-key-copy-paste/">(image courtesy)</a> </figcaption>
</figure>

## Build your Knowledge Tree
<div style="text-align: justify" style="padding-top: 5px; padding-bottom: 12px;"> 
Elon Musk在一次访谈中被问到“How do you learn so much so fast? ”，他是这样说的：
</div>
<div style="text-align: justify" class="quote_text"> 
"It is important to view knowledge as sort of a semantic tree — make sure you understand the fundamental principles, i.e. the trunk and big branches, before you get into the leaves/details or there is nothing for them to hang on to. "
 </div>


<figure>
<img src="/assets/img/nand2tetris/elon_musk_semantic_tree.jpg" alt="stack overflow keyboard" class="center" width="600"/>
<figcaption>Fig.2 - Build your knowledge tree <a href="https://www.reddit.com/r/GetMotivated/comments/2rsukr/image_elon_musk_on_learning_from_his_ama/">(image courtesy)</a></figcaption>
</figure>

<div style="text-align: justify" style="padding-top: 15px; padding-bottom: 12px;"> 
那对我们来说，这些知识的主干就是像计算机体系结构，组成原理，操作系统，编译原理，计算机网络等这些核心的计算机基础知识。北美很多名校的公开课比如麻省理工的6.S081，6.004, 斯坦福的CS110, CS107, CS144这些课程的质量都很棒，视频课件公开，只要一个一个课程作业及实验课跟着做完（我还没有），就能建立起完整的计算机领域的知识体系。但今天要讲的是另外一门神奇的课程Nand To Tetris 从与非门到俄罗斯方块。该课程从一个与非门开始，根据一套给定的简化指令集，构建一个完整的计算机 - HACK Computer, 并编写汇编器及编译器，最终用自己的一个编程语言(JACK)实现Tetris游戏。如课程教授所说，
 </div>

<div style="text-align: justify" class="quote_text"  style="padding-bottom: 8px;"> 
"Nand to Tetris courses are not restricted to computer science majors. Rather, they lend themselves to learners from any discipline who seek to gain a hands-on understanding of hardware architectures, operating systems, compilation, and software engineering - all in one course! "
 </div>

## Lecture resources

<div style="text-align: justify"  style="padding-bottom: 12px;"> 
这里是<a href="https://www.nand2tetris.org/">课程网站</a>，需要的软件工具，硬件仿真(Hardware simulator, CPU emulator, VM emulator)，课程资料，技术手册都在这里。 Cool Stuff这里展示了一些非常有趣的个人项目，有人不满足于本课程的HDL语言与仿真平台，直接把HACK电脑用FPGA实现了！有人实现了不可思议的光纤追踪。 
 </div>

<figure>
<img src="/assets/img/nand2tetris/cool_stuff.png" alt="cool project" class="center" width="600"/>
<figcaption>Fig.3 - Cool stuff <a href="https://www.nand2tetris.org/copy-of-talks">(image courtesy)</a></figcaption>
</figure>

<div style="text-align: justify" style="padding-top: 12px; padding-bottom: 12px;"> 
一门课覆盖如此全面的计算机基础知识。你可能会问，"Is it possible to cover so much ground in a one-semester course?" 答案是肯定的，证明就是全球已经有超过150所大学开设这门课程，学生们的满意度超乎寻常，MOOC平台Courera上的Nand to Tetris课程评分常年靠前。当然了，一个学期的课程涉及到计算机方方面面，广度自然牺牲掉了一部分深度。
</div>

<div style="text-align: justify" style="padding-top: 12px; padding-bottom: 12px;"> 
Coursera上的课程分为上下两部分，上部属于Hardware, 完成Hack Computer的搭建; 下部属于Software，完成从虚拟机到操作系统，编译器汇编器，高级语言设计及最后游戏的编码。我在去年完成了这门课的上半部分，在享受造物乐趣的同时: 在仿真器里，用一个很简单的HDL硬件描述语言，从一个门电路开始到最终完成所有模块的构建，Multiplexer, Demultiplexer, Register, RAM, ALU, CPU, 一步一步学习了解到了计算机的工作原理，there is no magic!
</div>


<a href="https://www.coursera.org/learn/build-a-computer">Nand to Tetris Part I</a>
<br>
<a href="https://www.coursera.org/learn/nand2tetris2">Nand to Tetris Part II</a>

Let’s fully understand what’s going on inside computers, and find the beauty of the picture at large!
<figure>
<img src="/assets/img/nand2tetris/build_understand.PNG" alt="Richard Feynman saying" class="center" width="600"/>
<figcaption>Fig.4 - Create then Understand</figcaption>
</figure>

<div style="text-align: justify" style="padding-top: 12px; padding-bottom: 12px;"> 
如作者所讲，通过这样一门综合全面的计算机系统课，我们可以避免只见树木不见森林的情况。头脑中建立起计算机系统的全貌，之后再有的放矢的加强领域知识学习效果会更好。
</div>

<div style="text-align: justify" class="quote_text"  style="padding-bottom: 12px;"> 
"We wrote this book because we felt that many computer science students are missing the forest for the trees. We believe that the best way to understand how computers work is to build one from scratch. "
 </div>


## Learning Roadmap 
<div style="text-align: justify" style="padding-top: 5px; padding-bottom: 12px;"> 
整个课程，正如路线图所展示的这样，总共需要完成12个硬件与软件构建任务。P1-P6完成HACK Computer(A simple but sufficiently powerful computer system)硬件的搭建；P7-P12完成从虚拟机，编译器，到操作系统的搭建。
</div>

硬件部分路线图
<figure>
<img src="/assets/img/nand2tetris/Roadmap-hardware-1.PNG" alt="nand2tetris roadmap hardware" class="center" width="600"/>
<figcaption>Fig.5- Roadmap on hardware <a href="https://www.nand2tetris.org/course">(image courtesy)</a></figcaption>
</figure>

软件部分路线图
<figure>
<img src="/assets/img/nand2tetris/Roadmap-software-2.PNG" alt="nand2tetris roadmap software" class="center" width="600"/>
<figcaption>Fig.6 - Roadmap on software <a href="https://www.nand2tetris.org/course">(image courtesy)</a></figcaption>
</figure>

<div style="text-align: justify" style="padding-top: 5px; padding-bottom: 12px;"> 
具体来说，通过12个项目，我们将完成以下的主题学习：
</div>
- Hardware: 布尔代数，组合逻辑电路，时序逻辑电路，各种逻辑门电路的实现，复用器，触发器，寄存器，RAM，计数器，硬件描述语言，芯片功能仿真验证与测试
- Architecture: ALU/CPU的设计与实现，时钟周期，寻址模式，取指与执行，指令集，内存映射的输入输出
- Low-level languages: 设计与实现一套简单的机器语言，指令集，汇编语言，汇编器
- Virtual machines: 自动机，堆栈设计，函数调用与返回，递归的处理，设计与实现一个简单的虚拟机语言
- High-level languages: 设计与实现一个简单的面向对象，与Java类似的编程语言，包括抽象数据类型，类，构造体，函数方法，语法语义，作用域，引用
- Compilers: 词法分析，符号表，代码产生，数组与对象的实现，分层编译
- Programming: 完成汇编器，虚拟机，编译器，API的设计
- Operating systems: 内存管理，数学计算库，输入输出设备的驱动，字符串处理，文本输出，图形输出，高级语言支持
- Data structure and algorithms: 栈，哈希表，链表，树，代数与几何算法，实时性考量
- Software engineering: 模块化设计， 接口与实现范式，API设计，写文档，单元测试，测试规范，软件质量，复杂系统设计

<div style="text-align: justify" style="padding-top: 5px; padding-bottom: 12px;"> 
从第一原理出发，通过仔细的推理和模块化规划，我们将从零到一搭建一台现代计算机系统，  在这个过程中我们也将学习到如何有效地规划和管理大型硬件和软件开发项目。
</div>


## Start the journey
<div style="text-align: justify" style="padding-top: 5px; padding-bottom: 12px;"> 
前几天，在亚马逊上看到了这门课配套书的第二版(2021)，马上下了单。估计中文版一时半会儿出不来。我就打算在完成下半部分的过程中，把新版的书过一遍，同时把学习过程分享在这个博客上，大家共同学习进步！
</div>

<figure>
<img src="/assets/img/nand2tetris/mybook.jpg" alt="my book" class="center" width="620"/>
<figcaption>Fig.7 - The Elements of Computing Systems</figcaption>
</figure>



<div style="text-align: justify" class="quote_text"  style="padding-bottom: 12px;"> 
"If you’ll manage to complete the journey, you will gain an excellent basis for becoming a hardcore computer professional yourself!"
 </div>
与君共勉




<html>
<head>
<style>
figure {
   text-align: center;
}

.img-container {
    text-align: center;
}

.quote_text
{
    font-size:      17px;
    font-style: italic;
}
        
figcaption {
  font-style: italic;
  padding: 2px;
  text-align: center;
}



</style>
</head>

</html>