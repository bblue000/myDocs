Android应用开发规范
===

- [命名规范](#specification_name)
	- [编程基本命名规范](#specification_name_basic)
	- [分类命名规范](#specification_name_detail)
	- [android常用控件命名规范](#specification_name_android)
- [其他开发规范](#specification_other)


<a name="specification_name"></a>

## 一、命名
为一个资源命名时，应该本着**描述性**以及**唯一性**这两大特征来命名，才能保证资源之间不冲突，且便于记忆。

名称应该说明“什么”而不是“如何”。

#### Keep In Mind：
	
> 使名称足够长以便有一定的意义，足够短以避免冗长



<a name="specification_name_basic"></a>
### 1.1. 编程基本命名规范

（1）类用帕斯卡命名法（例如：CalculateInvoiceTotal，每个单词的首字母大写），变量用驼峰式命名法（例如：documentFormatType，第一个单词首字母小写，后续单词首字母大写）；

（2）部分格式：

　　类前缀：项目名缩写（可选）；

　　私有成员变量：使用m开头，例如：mNameLabel；

　　常量：WORD1\_WORD2\_WORD3

（3）避免难懂的名称，如属性名xxK8，这样的名称会导致多义性。   
（4）在面向对象的语言中，在类属性的名称中包含类名是多余的，如Book.bookTitle，而是应该使用Book.title。   
（5）在允许函数重载的语言中，所有重载都应该执行相似的函数。
（6）使用动词-名词的方法来命名对给定对象执行特定操作的例程，如calculateInvoiceTotal()。（例程是某个系统对外提供的功能接口或服务的集合）  
（7）适当在变量名的末尾或开头加计算限定符（avg、sum、min、max、index）。 
（8）在变量名中使用互补对，如min/max、begin/end和open/close。  
（9）布尔变量名应该包含is，这意味着Yes/No 或 True/False 值，如 fileIsFound。  
（10）即使对于可能仅出现在几个代码行中的生存期很短的变量，仍然使用有意义的名  称。仅对于短循环索引使用单字母变量名，如 i 或 j。  
（11）不要直接使用数字或字符串，而是声明成命名常数，NUM_DAYS_IN_WEEK ，以便于维护和理解。 


<a name="specification_name_detail"></a>
### 1.2. 分类命名规范

#### （1）包的命名

　　Java包的名字都是由小写单词组成。

　　形如`<domain>.<project name>.<module>[.<sub package>]`

　　例如：

	com.google.guava.collect
	com.android.view

#### （2）类的命名

　　类的名字必须由大写字母开头，而单词中的其他字母均为小写；
	
　　如果类名称由多个单词组成，则每个单词的首字母均应为大写，例如：`TestPage`；

　　如果类名称中包含单词缩写，则这个所写词的每个字母均应大写，例如：`XMLExample`；

　　还有一点命名技巧就是由于类是设计用来代表对象的，所以在命名类时应尽量选择名词，例如：`Circle`。

#### （3）方法的命名

　　方法的名字的第一个单词应以小写字母作为开头，后面的单词则用大写字母开头。

　　例如： sendMessge

#### （4）常量的命名

　　常量的名字应该都使用大写字母，并且指出该常量完整含义。如果一个常量名称由多个单词组成，则应该用下划线来分割这些单词。
　　例如： MAX_VALUE

#### （5）参数的命名

　　参数的命名规范和方法的命名规范相同，而且为了避免阅读程序时造成迷惑，请在尽量保证参数名称为一个单词的情况下使参数的命名尽可能明确。

#### （6）Javadoc注释

　　Javadoc注释是一种多行注释，以/**开头，而以*/结束，注释可以包含一些HTML标记符和专门的关键词。

　　使用Javadoc注释的好处是编写的注释可以被自动转为在线文档，省去了单独编写程序文档的麻烦。行如：

	/**
	 * This is an example of
	 * Javadoc
	 *
	 * @author darchon
	 * @version 0.1, 10/11/2002
	 */

##### 建议：

>　　包、类、方法注释中建议包含**作者（`@author XXX`）**，**加入版本（`@version 1.0`）**；

>　　方法内部代码行注释（加入，修改代码），建议增加**“修改原因”**，**“修改人”**，**“修改时间”**等信息。

<br/>

　　**类注释：**

	/**
	 * 类名
	 * @author 作者 <br/>
	 * 实现的主要功能。
	 * 创建日期
	 * 修改者，修改日期，修改内容。
	 */
 
　　**方法注释：**

	/**
	 * 方法的一句话概述
	 * <p>方法详述（简单方法可不必详述）</p>
	 * @param s 说明参数含义
	 * @return 说明返回值含义
	 * @throws IOException 说明发生此异常的条件
	 * @throws NullPointerException 说明发生此异常的条件
	 */
 
 
　　成员变量和常量需要使用java doc形式的注释，以说明当前变量或常量的含义：

	/**
	 * XXXX含义
	 */
 
　　**方法内部的注释：**

　　如果需要多行，使用`/*…… */`形式，如果为单行，使用`//……`形式的注释。

　　不要在方法内部使用 javadoc 形式的注释`/**……**/`。

##### 总结：

　　在每个程序的最开始部分，一般都用Javadoc注释对程序的总体描述以及版权信息，之后在主程序中可以为每个类、接口、方法、字段添加Javadoc注释，每个注释的开头部分先用一句话概括该类、接口、方法、字段所完成的功能，这句话应单独占据一行以突出其概括作用，在这句话后面可以跟随更加详细的描述段落。在描述性段落之后还可以跟随一些以Javadoc注释标签开头的特殊段落，例如上面例子中的@auther和@version，这些段落将在生成文档中以特定方式显示。


<a name="specification_name_android"></a>
### 1.3. android相关规范

#### 1.3.1. android常用控件命名规范

|   组件    |   命名 |
|:---------:|:------:|
|TextView   |描述_TV |
|Button     |描述_BTN|
|ImageView  |描述_IV |
|CheckBox   |描述_CB |
|EditText   |描述_ET |
|ProgressBar|描述_PB |
|WebView    |描述_WV |
|ListView   |描述_LV |
|    ...    |  ...  |

例：mTitle_TV(TextView)， mProductDetail_IV(ImageView)，mContent_RL(RelativeLayout)。


<a name="specification_other"></a>
## 二、其他开发规范

1.java代码中不出现中文，注释代码可以使用中文。

2.关键代码添加注释。注释按照官方注释方式。

3.使用自己的LogUtils打印日志。

4.Class命名为: 业务名+功能名，例: MainActivity,ProductDetailActivity

5.全局变量命名: m+Name ，m之后的单词首字母大写。如果是View则在最后添加_下划线+控件简写的大写方式。

例：mTitle_TV(TextView)， mProductDetail_IV(ImageView)，mContent_RL(RelativeLayout)。

6.SVN/Git代码提交添加描述，描述此次提交修复的问题和作用。（尽量每天都进行一次提交，更新）

7.drawable中的图片命名：activity名称_逻辑名称/common_逻辑名称

8.不可以擅自引入第三方库。需严格限定第三方库大小，以及明确作用才可引用。
