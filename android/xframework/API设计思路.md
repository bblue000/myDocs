# API总设计思路



## 起因
对Android应用程序而言，不少API（例如任务，网络请求，下载，提示框等）是可以有不同的实现的；

对于一个框架而言，写死一种API是不周全的，强制式的（要知道山外有山，人外有人），不利于框架使用者自行扩展，比如，开发者既使用了ok-http，又使用了EventBus，universal-image-loader等开源库，而这些开源库内部都创建了一套线程池，或都使用/封装了异步任务机制，这对应用而言，不少代码是多余的，运行时的开销也是多余的；

因此，我想将这些常用的API暂且抽象出来，并分别提供默认实现。


## 设计


### 项目结构

起初，跟其他的开源库一样，all in one，掺杂在一起，beta1：

	┕ app
		┕ framework { API1|API2|... }
					 { API1-default|API2-default|... }

这样“不优雅”的同时，框架中被替换的API的默认实现也都会保留在了项目之中（在没有使用ProGuard优化的情况下）；

接着，优化为，beta2：

	┕ app
		┕ framework { API1|API2|... }
				┕ API1-default
				┕ API2-default
				...

虽说默认实现分开了，但是问题依然存在，在不让框架使用者修改framework的dependencies的情况下，效果跟上面一样；

再优化，beta3：

	┕ app
		┕ framework { API1|API2|... }
		┕ API1-default
		┕ API2-default
		...

让使用者像使用开关一样，可以把默认的实现“拔掉”，插上自己的“机器”运行，这样更具动态性，可扩展性也强。




### 接口替换方式

根据设计的项目结构，需要设计相应的使用情景，即框架使用者怎么使用和替换API。

对于项目结构beta1和beta2，给使用者提供的配置方式为：

	// API接口
	interface IAPI {
		void doSth();
	}

	// 对外开放的API类
	class API {
		private static IAPI sAPI;
		void setAPI(IAPI api) {
			sAPI = api;
		}

		public static void doSth() {
			sAPI.doSth();
		}
	}

但要让使用者以硬编码的方式写在Application中，总归不够优雅；

思来想去最后使用类似SPI（Service Provider Interface）的方式。


