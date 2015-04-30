# windowSoftInputMode属性 #

## 对单个界面

### stateXXXX
1. stateUnspecific
> 当设置属性为stateUnspecified的时候，系统是默认不弹出软键盘的；
> 
> 大
> 
> 但是当有获得焦点的输入框的界面有滚动的需求的时候，会自动弹出软键盘。
> 


*Tips:* 

 1. <u>让界面不自动弹出软键盘的其中一个解决方案，就是在xml文件中，设置一个非输入框控件获取焦点，从而阻止键盘弹出</u>
 
 是是

 2. 是

### adjustXXXX

设置软键盘与软件的显示内容之间的显示关系。

*注意：*

1. adjustUnspecified

 系统判断处理，

2. adjustNothing  

 任性，打死不动

3. adjustResize



4. adjustPan

 把控件举起来，保证你看得到。

 > **移动的前提：**软键盘弹出将会遮住控件
 > 
 > **移动的位置：**当且仅当控件“映入眼帘”


*结论:*

> 如果不设置"adjust..."的属性，对于没有滚动控件的布局来说，采用的是`adjustPan`方式，而对于有滚动控件的布局，则是采用的`adjustResize`方式


