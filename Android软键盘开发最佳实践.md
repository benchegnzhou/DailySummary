# Android开发软键盘最佳实践 #

## Android开发键盘把底部导航顶上去了解决办法 ##
解决办法，在mainfest.xml中，在和导航条相关的Activity中加：

  <activity
            Android:name=".filing.AddFilingActivity"
            android:windowSoftInputMode="adjustPan|stateHidden"
             />

 android:windowSoftInputMode="adjustPan|stateHidden"这个属性，问题解决：

属性解释：


:<activity android:windowSoftInputMode="stateVisible|adjustResize". . . >

在这设置的值(除"stateUnspecified"和"adjustUnspecified"以外)将覆盖在主题中设置的值


各值的含义：

|序号	|值		|			备注|
|-------|----------|----------|
|【A】		|stateUnspecified|软键盘的状态并没有指定，系统将选择一个合适的状态或依赖于主题的设置|
|【B】		|stateUnchanged|当这个activity出现时，软键盘将一直保持在上一个activity里的状态，无论是隐藏还是显示|
|【C】		|stateHidden|用户选择activity时，软键盘总是被隐藏|
|【D】		|stateAlwaysHidden|当该Activity主窗口获取焦点时，软键盘也总是被隐藏的|
|【E】		|stateVisible|软键盘通常是可见的|
|【F】		|stateAlwaysVisible|用户选择activity时，软键盘总是显示的状态|
|【G】		|adjustUnspecified|默认设置，通常由系统自行决定是隐藏还是显示|
|【H】		|adjustResize|该Activity总是调整屏幕的大小以便留出软键盘的空间|
|【I】		|adjustPan|当前窗口的内容将自动移动以便当前焦点从不被键盘覆盖和用户能总是看到输入内容的部分|


注意：

在activity.java文件中千万不要添加了如下代码：

getWindow().setSoftInputMode(
 WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_HIDDEN);
本行代码与配置文件中的设置产生了冲突，导致配置文件中代码不起作用！希望帮助每一个有相同问题的同志！



## 2. 在自己的最佳实践当中，直接的在XML中添加这段代码可以解决底部导航栏被弹起 ##

	Android:windowSoftInputMode="stateunspeciedadjustPan"

* 这种方法目前正在使用，是最有效果的一种解决方案，简单明了一句话解决问题


## 3. 除了上面的这一种自己实践外还有几种解决的方案是网络上面提出的，有机会的话可以尝试一下 ##
	
	当在Android的layout设计里面，如果输入文字的时候会底部控件会向上移，结果按照网上的说法找到以下三种方法：
	
	方法一：在你的activity中的oncreate中setContentView之前写上这个代码getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_PAN);
	方法二：在项目的AndroidManifest.xml文件中界面对应的<activity>里加入android:windowSoftInputMode="stateVisible|adjustResize"，这样会让屏幕整体上移。如果加上的是
	                android:windowSoftInputMode="adjustPan"这样键盘就会覆盖屏幕。
	方法三：把顶级的layout替换成ScrollView，或者说在顶级的Layout上面再加一层ScrollView的封装。这样就会把软键盘和输入框一起滚动了，软键盘会一直处于底部。
 
## 4. 当让在特定的需求下我们可能会需要软键盘弹起的时候，底部导航栏顺带着也要跟着弹起 ##

	 android:windowSoftInputMode="stateAlwaysHidden|adjustResize"




## 5. 还有另外的一种应用的场景，就是在软键盘弹起的时候，view滚动刀片指定的高度 ##

	 /**
	     *  1、获取main在窗体的可视区域
	     *  2、获取main在窗体的不可视区域高度
	     *  3、判断不可视区域高度
	     *      1、大于100：键盘显示  获取Scroll的窗体坐标
	     *                           算出main需要滚动的高度，使scroll显示。
	     *      2、小于100：键盘隐藏
	     *
	     * @param main 根布局
	     * @param scroll 需要显示的最下方View
	     */
	    public void addLayoutListener(final View main, final View scroll) {
	        main.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
	            @Override
	            public void onGlobalLayout() {
	                Rect rect = new Rect();
	                main.getWindowVisibleDisplayFrame(rect);
	                int mainInvisibleHeight = main.getRootView().getHeight() - rect.bottom;
	                if (mainInvisibleHeight > 100) {
	                    int[] location = new int[2];
	                    scroll.getLocationInWindow(location);
	                    int srollHeight = (location[1] + scroll.getHeight()) - rect.bottom;
	                    main.scrollTo(0, srollHeight);
	                } else {
	                    main.scrollTo(0, 0);
	                }
	            }
	        });
	    }
	 
* 这个试过，挺好用的
* 只是在rootView进行封装的前提下。就显得有些鸡肋了



