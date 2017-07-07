# 网上提供了一种解决Androidstudio卡顿的解决办法 #

[http://www.cnblogs.com/zhujiabin/p/5683024.html](http://www.cnblogs.com/zhujiabin/p/5683024.html)

## 参考链接二 ##

[https://www.zhihu.com/question/27953288](https://www.zhihu.com/question/27953288)


> 明确流程

* 先说一下 gradle 的生命周期吧，gradle 构建一个工程主要分为三部分（完全掌握了下面这张图，整个 gradle 的构建过程能了解个十之七八了）：
* 作者：知乎用户
* 链接：https://www.zhihu.com/question/27953288/answer/118031242
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

* 初始化阶段：主要是解析 setting.gradle 文件（因此有人提到减少 setting.gradle 的 module 数量，是很有道理的，但是实际操作过程限制颇多，原因最后会大致说一下）；读取配置阶段：主要是解析所有的 projects 下的 build.gradle 文件，包括 rootProject 和其他的 subprojects（子项目），检查语法，确定 tasks 依赖以建立 task 的有向无循环图，检查 task 里引用的文件目录是否存在等（这一步也进一步验证了减少 setting.gradle 里的 module 数量可以加快编译速度，因为减少一个 module ，需要解析的 build.gradle 文件就减少一个，第 3 步里就不会执行本属于这个 module 的任务了，但是还是 1 里面说的问题，限制颇多）；执行阶段：按照 2 中建立的有向无循环图来执行每一个 task ，整个编译过程中，这一步基本会占去 9 成以上的时间，尤其是对于 Android 项目来讲，将 java 转为 class compileDebugJavaWithJavac/compileReleaseJavaWithJavac
和 将 class 合并成 

		dex transformClassesWithDexForDebug/transformClassesWithDexForRelease

* 这两步很耗时，第一步还好，第二步会耗时非常久。首先在 gradle.properties 里设置 `org.gradle.jvmargs=-Xmx4096m` //越大越好
，然后在工程的 build.gradle 里的 android 结点下增加 dexOptions 配置，如下：
	
		dexOptions {
		    dexInProcess true
		    preDexLibraries true
		    javaMaxHeapSize "4g"//越大越好
		    incremental true
		}