# Android开发过程中so文件的引入和处理 #
# 1. 在日常开发中经常会使用到so文件库，也经常会遇到各种各样的问题 #

* 对应不同的市场发布的app你应当对应取舍的加入不同的so文件这样既保证了APP体积的总大小，同时一不至于这一类的手机运行崩溃
* 具体的配置信息参考一下的链接

[http://www.open-open.com/lib/view/open1440421271716.html](http://www.open-open.com/lib/view/open1440421271716.html)


> 本地参考文件: 
> **关于Android的.so文件不同手机架构的配置.oxps**



## 出现引入so文件后，部分手机报错(大多数是新版64位的手机)，其他手机运行正常的情况 ##

> * 出现问题的原因是64位的手机默认会选择对应的ARMv8 64位 ABIs加载so库，这时候如果这个文件存在将会在APP被安装的时候优先被安装，其他的文件捡不回被安装，这时候如果ARMv8中的so文件如果没有支持到64位或者干脆不存在对应的so文件，就会报错找不到so文件
> * 解决的办法无非就是讲对应的ARMv8 包去掉，但是这样会降低APP整体的性能
> * 另一种解决的思路就是，在文件中添加如下的代码，使得APP强制性的加载某几个目录下的so文件



> 具体的参考链接如下：
> [http://blog.csdn.net/vhawk/article/details/49964475](http://blog.csdn.net/vhawk/article/details/49964475)
 		
	

		解决方案也很简单，在build.gradle中加上，这句话的意思大概就是，告诉程序加载如下的so文件


		defaultConfig {
		  ......
		
		    ndk {
		        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
		    }
		
		
		}
		这时会报错，因为gradle的配置中，并不知道是否要做操作系统的兼容，这时只要在gradle.properties文件中加上
		
		Android.useDeprecatedNdk=true
		就可以了。
		
		运行，终于能奇迹般地运行起来了。

* 下面是博客的全文


		   猴子在调用高德地图的时候，遇到一个操作系统兼容问题，异常堆栈说是找不到so库文件，猴子就不懂了，明明有他说的那个文件。
		
		        后来和同行交流，他们的jniLibs中有一个arm64-v8a的文件夹用来存放64位操作系统的so库文件，于是猴子自己建了一个arm64-v8a文件夹，因为高德并没有把64位的so库文件公布出来，所以，猴子直接把他们唯一的so库考到了arm64-v8a文件夹下。自以为这下就该好了，于是等待见证奇迹。奇迹果然发生了，爆出了那个so库是32位而非64位的异常。猴子果断蒙圈了。下载了高德的demo，看到她们的demo里面也只有armeabi这一个库文件夹。这让猴子感觉到，64位的操作系统可以兼容32位的库文件。于是多方查找资料。终于在一家英文网站上找到了答案。
		
		       解决方案也很简单，在build.gradle中加上，这句话的意思大概就是，告诉程序加载如下的so文件
		
		
		defaultConfig {
		  ......
		
		    ndk {
		        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
		    }
		
		
		}
		这时会报错，因为gradle的配置中，并不知道是否要做操作系统的兼容，这时只要在gradle.properties文件中加上
		
		Android.useDeprecatedNdk=true
		就可以了。
		
		运行，终于能奇迹般地运行起来了。


# so文件的兼容和适配 #

> 文章链接参考：[http://blog.coderclock.com/2017/05/07/android/Android-so-files-compatibility-and-adaptation/](http://blog.coderclock.com/2017/05/07/android/Android-so-files-compatibility-and-adaptation/)


> 文章本地参考链接： **Android SO文件的兼容和适配.oxps**

![](http://i.imgur.com/YMMxHcD.png)

![](http://i.imgur.com/lUDcxhY.png)

![](http://i.imgur.com/9tZfQMg.png)

