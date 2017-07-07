# Android常见开源解决方案 #
原文参考链接： 
[http://m.toutiao.com/group/6348269082899497218/?iid=6241145735&app=news_article&wxshare_count=1&tt_from=weixin&utm_source=weixin&utm_medium=toutiao_android&utm_campaign=client_share](http://m.toutiao.com/group/6348269082899497218/?iid=6241145735&app=news_article&wxshare_count=1&tt_from=weixin&utm_source=weixin&utm_medium=toutiao_android&utm_campaign=client_share)


 
## （一）系统框架 ##

### 1.1 网络请求 ###

|名称|	描述|
|------|-------|
|Android Async HTTP|	Android异步HTTP库|
|AndroidAsync	|异步Socket，HTTP(客户端+服务器)，WebSocket，和socket.io库。基于NIO而不是线程。|
|OkHttp	|一个Http与Http/2的客户端|
|Retrofit	|类型安全的Http客户端|
|Volley|	Google推出的Android异步网络请求框架和图片加载框架|



### 1.2 网络解析 ###

* **1.2.1 JSON解析**

|名称	|描述|
|--------|-------|
|Gson	|一个Java序列化/反序列化库，可以将JSON和java对象互相转换|
|Jackson	|Jackson可以轻松地将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象|
|Fastjson	|Java上一个快速的JSON解析器/生成器|




* **1.2.2 HTML解析**

|名称|	描述|
|------|-----|
|HtmlPaser	|一种用来解析单个独立html或嵌套html的方式|
|Jsoup	|一个以最好的DOM，CSS和jQuery解析html的库|


* **1.3 图片加载**

|名称|	描述|
|------|-------|
|Android Universal Image Loader	|一个强大的加载，缓存，展示图片的库|
|Picasso	|一个强大的图片下载与缓存的库|
|Fresco	|一个用于管理图像和他们使用的内存的库|
|Glide	|一个图片加载和缓存的库|




* **1.4 图片处理**

|名称|	描述|
|------|-------|
|Picasso-transformations	|一个为Picasso提供多种图片变换的库|
|Glide-transformations	|一个为Glide提供多种图片变换的库|
|Android-gpuimage	|基于OpenGL的Android过滤器|




* **1.5 图表处理**

|名称|	描述|
|------|-------|
|WilliamChart|	创建图表的Android库|
|HelloCharts|	兼容到API8的Android图表库|
|MPAndroidChart	|一个强大的Android图表视图/图形库|





* **1.6 后台处理**

|名称|	描述|
|------|-------|
|Tape	|一个轻快的，事务性的，基于文件的FIFO的库|
|Android Priority Job Queue	|一个专门为Android轻松调度任务的工作队列|




* **1.7 响应式编程**

|名称|	描述|
|------|-------|
|RxJava	|JVM上的响应式扩展|
|RxJavaJoins	|为RxJava提供Joins操作|
|RxAndroid	|Android上的响应式扩展，在RxJava基础上添加了Android线程调度|
|RxBinding	|提供用RxJava绑定Android UI的API|
|Agera	|Android上的响应式编程|




* **1.8 事件总线**

|名称|	描述|
|------|-------|
|EventBus	|安卓优化的事件总线，简化了活动、片段、线程、服务等的通信|
|OTTO	|一个基于Guava的增强的事件总线|



* **1.9 缓存**

|名称|	描述|
|------|-------|
|DiskLruCache	|Java实现基于LRU的磁盘缓存|



* **1.10 数据库**

|名称|	描述|
|------|-------|
|OrmLite	|JDBC和Android的轻量级ORM java包|
|Sugar	|用超级简单的方法处理Android数据库|
|GreenDAO	|一种轻快地将对象映射到SQLite数据库的ORM解决方案|
|ActiveAndroid	|以活动记录方式为Android SQLite提供持久化|
|SQLBrite	|SQLiteOpenHelper 和ContentResolver的轻量级包装|
|Realm	|移动数据库，一个SQLite和ORM的替换品|


* **1.11 依赖注入**

|名称|	描述|
|------|-------|
|ButterKnife|	将Android视图和回调方法绑定到字段和方法上|
|Dagger2	|一个Android和java快速依赖注射器。|
|AndroidAnotations	|快速安卓开发。易于维护|
|RoboGuice|	Android平台的Google Guice|




* **1.12 Log框架**

|名称|	描述|
|------|-------|
|Logger	|简单，漂亮，强大的Android日志工具|
|Hugo|	在调试版本上注解的触发方法进行日志记录|
|Timber	|一个小的，可扩展的日志工具|


* **1.13 测试框架**

|名称|	描述|
|------|-------|
|Mockito	|Java编写的Mocking单元测试框架|
|Robotium	|Android UI 测试|
|Robolectric	|Android单元测试框架|
|Android自带很多测试工具|	JUnit，Monkeyrunner，UiAutomator，Espresso等|




* **1.14 调试框架**

|名称|	描述|
|------|-------|
|Stetho	|调试Android应用的桥梁，使得可以利用Chrome开发者工具进行调试|







* **1.15 性能优化**

|名称|	描述|
|------|-------|
|LeakCanary|	内存泄漏检测工具|
|ACRA|	Android应用程序崩溃报告|








### （二）控件库  ###

* **2.1 下拉刷新、上拉加载**

|名称|	说明|
|--------|--------|
|Android PullToRefresh	|最经典、最多人用的下拉刷新、加载更多。|
|Android Ultra Pull To Refresh	|非常方便自定义的下拉刷新框架，继承于ViewGroup可以包含任何View，使用起来非常简单，如果想要自定义样式，就像给ListView加一个HeaderView一样简|
|Phoenix	|漂亮的动态刷新库|
|Pull-to-Refresh.Rentals-Android	|一个简单可以自定义的下拉刷新实现，Yalantis 出品。|
|Taurus	|下拉刷新，Yalantis 出品。（是不是有点似曾相识呢？）|
|swipeRefreshLayout|	谷歌官方自带|






* **2.2 底部导航栏**

|名称|	说明|
|--------|--------|
|LuseenBottomNavigation|	很方便的遵循MD设计的底栏|
|ahbottomnavigation	|遵循谷歌设计规范的底栏库|
|BottomBar|	另一个遵循谷歌设计规范的底栏库（PS：之前我使用的时候有点小BUG现在不清楚）|



* **2.3 二维码**

|名称|	说明|
|--------|--------|
|Zxing|	谷歌开发的二维码处理库，功能强大，质量没的说！一维码二维码都能扫！但是使用起来比较繁琐。|
|ZBar	|ZBar则比较简单，使用起来容易上手|

- ZXing项目的示例程序对于摄像头的控制写的非常全面，ZBar的没有

- ZBar基于C语言编写，解码效率高于ZXing项目

- ZBar是日本人写的，对于中文解析会乱码这个肯定有人遇到过的，ZXing不会乱码

- 扫描框的绘制，ZXing的扫描框绘制是自定义View的，截取区域不好控制（至少我没控制好），ZBar的可以自定义，只要你会计算截取区域


----------------------


* **2.4 侧滑菜单**

|名称|	说明|
|--------|--------|
|NavigationDrawer	|Material Design的抽屉模板库，快速搭建侧滑菜单。|
|SlidingMenu	|之前也是个很优秀的侧滑库直到谷歌来搅局。。|
|FlowingDrawer|	超帅的侧滑菜单|




* **2.5 侧滑返回**

|名称|	说明|
|--------|--------|
|ParallaxSwipeBack|	带视觉差的侧滑返回，类似于新版微信和lofter的侧滑返回效果。引入方便，核心代码只需几十行！|



* **2.6 文本输入框**

|名称|	说明|
|--------|--------|
|MaterialEditText	|方便自定义的EditText控件库|


* **2.7 侧贴标签**

|名称|	说明|
|--------|--------|
|labelview	|类似淘宝的侧贴标签库|
|FlycoLabelView|	轻量级侧贴标签|



* **2.8 侧滑功能项**

|名称|	说明|
|--------|--------|
|AndroidSwipeLayout	|滑动Layout，支持单个View，ListView，GridView|



* **2.9 动画库**

|名称|	说明|
|--------|--------|
|animate	|几乎涵盖所有MD动画|
|Material-Animations	|MD动画库|
|ExplosionField	|爆破效果的删除动画库|
|LoadingDrawable	|小清新的加载动画|



* **2.10 搜索框**

|名称|	说明|
|--------|--------|
|floatingsearchview	|自带动画与动态加载数据的搜索框控件|


* **2.11 快捷选择**

|名称|	说明|
|--------|--------|
|drag-select-recyclerview|	一直觉得魅族的快捷选择很有用，终于找到一个类似功能的还支持recycleView|




* **2.12 引导页**

|名称|	说明|
|--------|--------|
|material-intro	|MD风格的引导页|
|SlidingTutorial|	很顺滑的引导页|
|MaterialIntroView|	MD式的高亮引导页|
|HTextView	|其实这是个帅炸的文字展示库，可是我用过做引导页而且逼格不错！|



* **2.13 菜单**

|名称|	说明|
|--------|--------|
|android-snake-menu|	类似Tumblr的蛇形菜单|
|coolMenu|	帅炸的视窗菜单|
|BoomMenu	|爆炸式的弹出菜单|



* **2.14 图案密码解锁单**

|名称|	说明|
|--------|--------|
|android-lockpattern	|Android开机的图案密码解锁，支付宝的密码解锁|


* **2.15 滑动开关**

|名称|	说明|
|--------|--------|
|ToggleButton	|状态切换的 Button，类似 iOS，用 View 实现|



* **2.16 对话框**

|名称|	说明|
|--------|--------|
|sweet-alert-dialog	|一个带动画效果的自定义对话框样式|




* **2.17 进度条**

|名称|	说明|
|--------|--------|
|NumberProgressBar	|个简约性感的数字进度条|
|CircularProgressView	|自定义view的方式实现的Material风格的加载提示控件，兼容任何版本。|


* **2.18 列表**

|名称|	说明|
|--------|--------|
|StickyListHeaders	|拼音字母悬停，GroupName滑动到顶端时会固定不动直到另外一个GroupName到达顶端的ExpandListView，支持快速滑动，支持Android2.3及以上|

- 下拉刷新上拉加载
- 侧滑功能
- 拖动排序


参考文章：
1、Android常用库之遇见你真舒心
2、15 个 Android 通用流行框架大全
3、Android开源项目第一篇——个性化控件(View)篇
4、2015最流行的Android组件、工具、框架大全
如果需要看源码请移步到我的博客直接点击连接即可：

[http://blog.csdn.net/geofferysun/article/details/53008509](http://blog.csdn.net/geofferysun/article/details/53008509)

欢迎大家继续补充~