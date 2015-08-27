## 电商SDK Android文档 ##


### 多APP根据自身设计选择定制深度 ###
> - 基本布局如无变动，尽量修改style变换风格，如间距，字体颜色，字体大小，按钮风格等等（详见[电商SDK界面基本风格整改](#m_style)）；
> 
> - 布局变动，但新添加的UI元素是`静态的`（即不需要从现有的model 里去获取数据），这种改动可以直接在app层面，简单修改xml；
> 
> - UI新增功能组件，如`Button`等，需要的是已有功能或添加简单功能，此时需要重写相应的`Activity`，复用方法（详见[Activity/Fragment的扩展修改](#m_activity_extends)）；
>
> - UI新增功能组件，且API接口参数/返回数据改变（需要配合修改该API的`Param`及`Result`数据），需要重写`Activity`并获取组件，绑定数据（详见[API的`Param`及`Result`数据修改](#m_model)、[Activity/Fragment的扩展修改](#m_activity_extends)）
> 
> - 界面流程改变，需要增加/删除/修改流程的，需要配合重写`Controller`（详见[界面流程修改](#m_controller_flow)）


此外，有关于Activity/Fragment顶部TitleBar的使用（详见[默认TitleBar的使用](#m_titlebar)）。




<br/>
<br/>
<br/>

<p id="m_style"/>
### ·电商SDK界面基本风格整改 ###
> SDK整理出一套通用的UI style，比如几种基本的字体样式（颜色和大小），按钮样式，边框样式及间距等等。
> 
> 多APP可以通过改变这些基本的样式，简单整改界面风格。




<br/>
<br/>
<br/>
<p id="m_activity_extends"/>
### ·Activity/Fragment扩展修改 ###
> SDK中为`Activity/Fragment`提供了规范基类——`BaseActivity/BaseFragment`，分别将`Activity`的`onCreate(Bundle`)和`Fragment`的`onActivityCreated(Bundle)`拆分为以下几个方法：
> 
> - `initView`-->初始化`View`，一系列`findViewById`操作；
> 
> - `initListener`-->初始化`View`的监听器，setOnXXXListener等；
> 
> - `initBroadcast`-->初始化广播，已经由基类实现，子类只需要通过`provideBroadcastActions()`方法提供需要监听的广播Action，并在`onReceiveBroadcast(String, Intent)`中监听即可；
> 
> - `initData`-->初始化数据，绑定默认/缓存数据，同时异步请求新数据；
> 
> （*以上方法都是初始化方法，后续的更新界面、监听器不一定可以直接使用它们*）
>
> SDK中的所有界面功能（按钮事件、UI绑定数据等）在编写过程中已经尽量**碎片化**，使得多APP在使用时尽可能直接覆写方法来实现定制修改，覆写方式例如：
> 
>     public class ActivityA extends BaseActivity {
>         ...
>         @Override
>         protected void initView(Bundle savedInstanceState) {
>             ...
>         }
>         
>         @Override
>         protected void initData(Bundle savedInstanceState) {
>             ...
>         }
>         
>         protected void updateDataToUI(Model model) {
>             ...
>         }
>         
>         public/protected void methodA() {
>             ...
>         }
>         ...
>     }
>     
>     public class ExtActivityA extends ActivityA {
>         ...
>         protected TextView newView;
>         @Override
>         protected void initView(Bundle savedInstanceState) {
>             super.initView(savedInstanceState);
>             newView = (TextView) findViewById(R.id.XXX); // 新查找的View
>         }
>         
>         @Override
>         protected void initData(Bundle savedInstanceState) {
>             super.initData(savedInstanceState);
>         }
>         
>         @Override
>         protected void updateDataToUI(Model model) {
>             super.initData(savedInstanceState);
>             newView.setText(model.getXXX()); // 给新查找的View绑定值
>         }
>         
>         @Override
>         public/protected void methodA() {
>             // do extra things
>             ... // change default operations
>             // do extra things
>         }
>         ...
>     }
>     
> 修改完成后，在相应的`ControllerConfig`中替换相关的UI流程配置（详见[界面流程修改](#m_controller_flow)）。



<br/>
<br/>
<br/>
<p id="m_model"/>
### ·API的`Param`及`Result`数据修改 ###
> 未完待续...




<br/>
<br/>
<br/>
<p id="m_controller_flow"/>
### ·界面流程修改 ###
> SDK中流程的控制由一系列Controller控制，Controller都有ControllerConfig配置具体的Controller类，如果多APP重写（继承）某个Controller需要在ControllerConfig中修改默认配置。
> <br/><br/>
> 
> 
> **API请求响应处理流程的修改**：
> 
> 在SDK中，UI调用Controller中的方法请求数据，Controller向Manager发送异步请求，并处理Manager的响应结果；
> 
> 所有响应结果的处理（成功和失败）都封装成单独的方法写在Controller中，便于多APP替换和扩展。
> 
> **Activity跳转流程的修改**：
> 
> SDK中界面的跳转由Controller提供方法控制。基于Android中Activity的跳转方式，SDK使用配置文件XXXControllerConfig为XXXController提供其流程中的相关Activity的类型（class）。
> 
> 新增/修改/删除UI跳转流程，只需要覆写Controller中相应的方法即可。
> 




<br/>
<br/>
<br/>
<p id="m_titlebar"/>
### ·默认TitleBar的使用 ###
> SDK中实现了一套默认的TitleBar工具——SimpleTitleBar，是基于“同一个APP使用同一种顶部布局风格”的思想。
> 
> BaseActivity和BaseFragment中TitleBar的开关和获取：
> 
>     public class BaseActivity extends FragmentActivity {
>         /**
>          * @return 是否需要获取默认的Titlebar
>          */
>         protected boolean showDefaultTitlebar() {
>             return true;
>         }
>         
>         /**
>          * 返回根布局中的titlebar控制器
>          *
>          * @return 如果{@link #showDefaultTitleBar()}返回false，则为null
>          */
>         protected SimpleTitleBar getDefaultTitleBar() {
>             ...
>         }
>         ...
>     }
>     
> 默认情况下，BaseActivity和BaseFragment中的`showDefaultTitlebar()`返回`true`。
> 
> 多APP也可以通过改变样式、布局、方法来自定义TitleBar。
> 
> 



