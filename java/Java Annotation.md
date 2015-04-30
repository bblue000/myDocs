

##1. Java Annotation因何而来？

最初从印象中，是可以替代之前JDK1.4开发中，大量繁琐的配置项，Annotation的出现其实可以极大简化配置文件的数量和需要关注配置的内容。在阅读了诸多文章之后，发掘，这个取代配置项只是一个附带的结果，或者是我们开发者从中受益最多的方面罢了。其实，它背后的原因远不是如此。

我们还是引用Wiki百科的观点来综述原因吧，**`a form of syntactic metadaa that can be added to Java source code`**，就是说，Annotation的引入是为了从Java语言层面上，为Java源代码提供元数据的支持[[java annotation](http://en.wikipedia.org/wiki/Java_annotation)]。

那第二个问题就来了，何为**`metadata`**(元数据)呢 ?

Wiki官方的解释是**`Metadata is "data about data"`**，精辟呀，用来描述数据本身的数据。如果我们把Java的源代码看做是数据的话，那么metadata就是为了描述这些源代码而产生和使用的元数据，Annotation就是在Java语言层面上实现了metadata机制。

Metadata分为两个方面的内容：   **structural metadata**和**descriptive metadata**，简而言之，就是结构性的元数据和描述性的元数据两种，当然这里的两种划分是否准确有待商榷，但是却让大致明白了它所应用的场景和场合。

那是否有童鞋依然对Metadata一头雾水呢? 下面我们来举几个小例子，帮助大家感知一下元数据吧。

比如我们从网上搜索一部电影的信息，电影本身是一个数据，可以播放和观看娱乐消遣。但我们如何才能找到我们想要的电影呢？想一想，对了，我们可以按照主演、导演、上映时间、影片类型、观众评分、票房收入和发行公司等诸多的信息进行搜索。这些我们搜索的条件，就是我们这里所谓的关于电影本身的metadata，他们都是用来描述电影本身的数据，但是不影响电影本身的播放和观看的。



##2.   Java Annotation的历史简述

在引入Annotation之前，Java中其实已经有了类似的东西，比如**`transient`**和**`@deprecated`**。在2004年正式被JCP接受，在JDK5中正式引入的，主要是通过开发包中的apt命令来进行处理。 在JDK6中，将其集成到javac， 允许用户自定义Annotation，为用户自行扩展开启了通道。


##3.   Annotation在Java中的应用

Annotation在Java可以像public, final等语法修饰一样使用， 用以修饰用于包、类 型、构造方法、方法、成员变量、参数、本地变量的声明中。另外对于允许自定义参数的Annotation还可以在声明中使用参数。

在Java中主要用在以下几个方面：

- 文档编制

 > 通过@Documented来标注是否需要在javadoc中出现。

- 编译器检查
 > 通过Annotation的使用，可以调整和控制编译器的使用以及让编译器提供关于代码的更多的检查和验证，比如`@Override`,`@SuppressWarning`.


- 代码分析
 > 这个是我们开发者从中受益良多的部分，通过Annotation的使用，可以让我们在代码运行中动态得去控制系统的行为，从而省去之前诸多的配置和冗余代码。这个会在后续的实例中加以详述。 这里有一点需要说明的是，Annotation不影响已有代码的执行，但是会影响系统在运行中的行为，这两个在不同的层面上，一个是已有的代码执行层，另外一个是JVM会根据Annotation的指令修改系统行为的。


##4.  Annotation在Java中的使用方法

下面我们来直接看一个Annotation的例子，感受一下它的威力吧。

[java]

	public class AnnotationOverrideTest {  
		@Override public String toString() { /////---  可以当做修饰符一样直接使用,非常熟悉吧  
			return "Override the toString() of the superclass";  
		}  
  
		@Override  //也可以分为单独一行  
		public String toString123() {  //提示编译错误  
			return "Override the toString() of the superclass";  
		}  
  
	}   

大家可以看到@+Annotation的名称就可以直接使用了，这里使用了@Override这个Annotation来让编译器检查toSring()这个方法是否覆盖了基类的方法。如果基类并没有这个方法的话，则会报错。 在toString123()这个方法中，就会提示错误信息：

[java] 

	AnnotationOverrideTest.java:7: method does not override or implement a method from a super-type  
		@Override public String toString123() {  
		^  
    
这个@Override可以帮助我们的攻城师尽可能早的发现代码中隐藏的问题，如果未使用它的话，可能只有在运行过程中，才会有机会发现这个问题，不是吗？

（未完待续， 后续提纲如下，欢迎指正）

##5.  Annotation在Java语言中的内置类型介绍

##6.  Annotation的类型分类

##7.  Annotation的原理实现

##8.  如何自定义Annotation

##9.  Annotation在Spring中的应用案例分析

##10.  总结