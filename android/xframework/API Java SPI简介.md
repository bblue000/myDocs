
# Java SPI
> A service provider is a factory for creating all known implementations of a particular class or interface S. 
> 
> The known implementations are read from a configuration file in `META-INF/services/`.

see [ServiceLoader](http://developer.android.com/reference/java/util/ServiceLoader.html)

<br/>

---
一句话理解：**使用指定的配置文件，动态加载服务实现类**;

比如，在java中charset中，`Charset.forName`中其实就使用到了类似功能，首先从缓存的对象中查找，再从本地查找，最终看用户有无自定义的`CharsetProvider`：

	// Does a configured CharsetProvider have this charset?
    for (CharsetProvider charsetProvider : ServiceLoader.load(CharsetProvider.class)) {
    	cs = charsetProvider.charsetForName(charsetName);
    	if (cs != null) {
    		return cacheCharset(charsetName, cs);
    	}
    }


<br/>

---
**使用步骤：**

编写好服务的接口/抽象类，如foo.Bar；

在java项目的src目录下创建`META-INF/services/foo.Bar`文件，如服务实现类为`foo.BarImpl.java`，上述文件中的内容就为`foo.BarImpl`；

在项目中使用`ServiceLoader`或者`ClassLoader.getResource("META-INF/services/foo.Bar")`就能获得实现类。





