#  ArcGIS Runtime for Android开发教程 #

* 参考文档链接：
[http://blog.csdn.net/arcgis_mobile/article/details/8263283](http://blog.csdn.net/arcgis_mobile/article/details/8263283)


![](http://i.imgur.com/8rq6Jzm.png)

## 1.  ArcGIS的初始化  ##

	 private void initMapView() {
	        // 设置授权信息（目的：去除水印）
	        ArcGISRuntime.setClientId("1G3512J1UJmwHvHq");
			// 地理坐标转换web墨卡托
	        double[] xxx = CoordinateFunction.lonLat2Mercator(default_x, default_y);
	        Point mapPoint = new Point(xxx[0], xxx[1]);

			// 获取 以mapPoint为中心的矩形 长 2000 宽 2000
	        Envelope evn = new Envelope(mapPoint, 2000, 2000);
	        mapView.setExtent(evn);
	
	        String tile_url = "http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=7";
	        //String tile_url = "http://192.168.74.0:8080/MapServer/Service?server=MapTile&map=default"
	        // Add a MBTilesLayer on top with 50% opacity

			//展示最底层的地图数据层
	        ZTTiledLayer mbLayer = new ZTTiledLayer(tile_url);
	        //mbLayer.setOpacity(0.5f);
	        mapView.addLayer(mbLayer);
	        // enable map to wrap around
	        mapView.enableWrapAround(true);
	



			//-----------------------------下面的是在原有地图上面的标志的绘制了-----------
	      
			//系统自带图标样式标识绘制
	        drawDefaultIcon();
	
			// 自定义图标样式的绘制
	  		drawCustomIcon();
 
 
	    }


## ArcGIS是使用图层的形式向地图添加数据的因此需要创建并向mapview里面添加图层 ##

			//系统自带图标样式标识绘制
	        drawDefaultIcon();
	
			// 自定义图标样式的绘制
	  		drawCustomIcon();




## drawDefaultIcon中的代码 ##

  	/**
     * 绘制系统自带的图标
     */
    private void drawDefaultIcon() {
		//创建一个新的图层
        GraphicsLayer all_feature_layer = new GraphicsLayer();
		//将图层添加到mapview
        mapView.addLayer(all_feature_layer);
		//获取 新添加图层对应的ID，留作备用
        all_feature_layer_id = all_feature_layer.getID();
    }


## drawCustomIcon中的代码 ##
 	/**
     * 自定义图片显示点
     */
    private void drawCustomIcon() {
		//创建一个新的图层
        GraphicsLayer all_feature_layer2 = new GraphicsLayer();
        //设置图层的透明度
        all_feature_layer2.setOpacity(50);
		//将图层添加到mapview
        mapView.addLayer(all_feature_layer2);
		//获取 新添加图层对应的ID，留作备用
        all_feature_layer_id2 = all_feature_layer2.getID();
    }



## 2. 使用 ArcGIS mapView系统样式 画点、十字、菱形、正方形、X形、三角形标识 ##

* ArcGIS 系统自身支持：6中样式的点标记

*  SimpleMarkerSymbol.STYLE.CIRCLE

|名称		|  	 样式	|
|-----------|:-----------:|
|   CIRCLE	|		圆	|
|     CROSS	|	十字架	|
|    DIAMOND|	菱形		|
|    SQUARE	|	正方形	|
|       X	|		X形 	|
|      TRIANGLE|三角形	|

*  **创建 SimpleMarkerSymbol，并指定SimpleMarkerSymbol标记的颜色，大小，样式**  

 	`SimpleMarkerSymbol mRedMarkerSymbol = new SimpleMarkerSymbol(Color.RED, 15, SimpleMarkerSymbol.STYLE.CIRCLE); `



* **地理坐标转换外围墨卡托坐标**
 
		   double[] xxx = CoordinateFunction.lonLat2Mercator(locationX,locationY); 
           Point mapPoint = new Point(xxx[0], xxx[1]);


*  **创建图层**
   
   	 	Graphic graphic = new Graphic(mapPoint, mTestRedMarkerSymbol);

* **通过ID获取图层**
 		
   		GraphicsLayer tmpLayer = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id);

* **将创建的标记添加到对应的图层**
   	
		tmpLayer.addGraphic(graphic);







## 监听设置 ##
### 1. 使用 `mapView.setOnSingleTapListener(onSingleTapListener);` 创建单击监听 ###

  OnSingleTapListener onSingleTapListener = new OnSingleTapListener() {
        @Override
        public void onSingleTap(float v, float v1) {
            Point pt = mapView.toMapPoint(v, v1);

            GraphicsLayer tmp_layer = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id2);
            int[] ids = tmp_layer.getGraphicIDs(v, v1, 2);

			//因为这些标志很有可能是重合的，一次性的可能点击有多个的坐标的情况，这样的时候就是需要将这些点遍历，然后尽心逻辑的处理
            for (int id : ids) {
				//  根据id获取对应的 Graphic 层    
                Graphic tmp = tmp_layer.getGraphic(id);

				//这些属性是通过	 Graphic graphic2 = new Graphic(mapPoint, pictureMarkerSymbol, attr);
				//构造传递过来的
                String feature_id = tmp.getAttributeValue("name").toString();


                double y = Double.parseDouble(tmp.getAttributeValue("y").toString());
                double x = Double.parseDouble(tmp.getAttributeValue("x").toString());

                double[] xxx = CoordinateFunction.lonLat2Mercator(x, y);

                Point mapPoint = new Point(xxx[0], xxx[1]);

                mapView.zoomTo(mapPoint, 4);

                ToastUtils.showToastShort(tmp.getAttributeValue("name").toString());
                break;


            }
 
        }
    };





### 2. 使用 `mMapView.setOnZoomListener(onZoomListener);` 创建缩放时监听 ###

	OnZoomListener onZoomListener = new OnZoomListener() {
	        @Override
	        public void preAction(float v, float v1, double v2) {
	
	        }
	
	        @Override
	        public void postAction(float v, float v1, double v2) {
				//获取地图的中心点对应其web墨卡托的坐标
	            Point pt = mMapView.getCenter();   
				
	            String msg = String.format("x:%f--y:%f", pt.getX(), pt.getY());
	            ToastUtils.showToastShort(msg);
	
	        }
	    };


* **注意区别三种概念**
- 地理坐标： 范围 -180 ~180
- web墨卡托坐标在： 百万级别
- 屏幕坐标： 这个既不用说了呵


### 3. 使用 `mMapView.setOnPanListener(onPanListener);` 创建平移时监听 ###

	 //为地图添加一个平移监听器
	    OnPanListener onPanListener = new OnPanListener() {
	        public void prePointerUp(float fromx, float fromy, float tox, float toy) {
	        }
	
	        public void prePointerMove(float fromx, float fromy, float tox, float toy) {
	        }
	
	        public void postPointerUp(float fromx, float fromy, float tox, float toy) {
	            Point pt = mMapView.getCenter();
	
	            String msg = String.format("x:%f--y:%f", pt.getX(), pt.getY());
	            ToastUtils.showToastShort(msg);
	        }
	
	        public void postPointerMove(float fromx, float fromy, float tox, float toy) {
	        }
	    };



## Graphic 绑定参数 ##
* **1. 使用map传递参数**

 			HashMap attr = new HashMap();
            attr.put("graphicId", newFeature.getFeatureId());
            attr.put("name", newFeature.getFeatureName());
            attr.put("x", newFeature.getFeatureX());
            attr.put("y", newFeature.getFeatureY());

* **2. 在创建图形对象 Graphic 的时候使用三个参数的构造可以将参数绑定到对应的Graphic**
  
		Graphic graphic2 = new Graphic(mapPoint, pictureMarkerSymbol, attr);

* **3. 图形被点击的时候获取参数**

	 	OnSingleTapListener onSingleTapListener = new OnSingleTapListener() {
	        @Override
	        public void onSingleTap(float v, float v1) {
	            Point pt = mapView.toMapPoint(v, v1);
	  			GraphicsLayer tmp_layer = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id2);
				//图形可能会重叠，有一次点击到多个的情况，获取到的将是点击到位置对应的所有
	            int[] ids = tmp_layer.getGraphicIDs(v, v1, 2);
	
	            for (int graphicId : ids) {
	                Graphic tmp = tmp_layer.getGraphic(graphicId);

					//通过ID获取属性
	                String feature_id = tmp.getAttributeValue("graphicId").toString();
	
	
	                double y = Double.parseDouble(tmp.getAttributeValue("y").toString());
	                double x = Double.parseDouble(tmp.getAttributeValue("x").toString());
					//
	                double[] xxx = CoordinateFunction.lonLat2Mercator(x, y);
	
	                Point mapPoint = new Point(xxx[0], xxx[1]);
					//将地图平移到以 mapPoint为中心的位置
	                mapView.zoomTo(mapPoint, 4);
					//输出图形上绑定的变量值
	                ToastUtils.showToastShort(tmp.getAttributeValue("name").toString());
	                break;
	
	
	            }
			}
		}

-----

## 附完整代码 ##


 
			/**
			 * 小区地图
			 * createby:zbc
			 */
			public class CommunityZoneMapActivity extends AppCompatActivity {
			
			    @Bind(R.id.mapView)
			    MapView mapView;
			
			
			
			    // 南京
			    double default_x = 118.761691;
			    double default_y = 32.068252;
			
			    private String zoneID = UserInformationManager.getInstance().getUserCurrentVillageID();;
			    private long all_feature_layer_id;
			
			    SimpleMarkerSymbol mRedMarkerSymbol = new SimpleMarkerSymbol(Color.RED, 15, SimpleMarkerSymbol.STYLE.CIRCLE);
			
			    SimpleMarkerSymbol mBlackMarkerSymbol = new SimpleMarkerSymbol(Color.BLACK, 80, SimpleMarkerSymbol.STYLE.CIRCLE);
			
			    SimpleMarkerSymbol mGreenMarkerSymbol = new SimpleMarkerSymbol(Color.GREEN, 80, SimpleMarkerSymbol.STYLE.CIRCLE);
			    SimpleMarkerSymbol mGrayMarkerSymbol = new SimpleMarkerSymbol(Color.GRAY, 80, SimpleMarkerSymbol.STYLE.CIRCLE);
			
			    SimpleLineSymbol mBlueLineSymbol = new SimpleLineSymbol(Color.BLUE, 1);
			
			    SimpleMarkerSymbol mTestRedMarkerSymbol = new SimpleMarkerSymbol(Color.RED, 10, SimpleMarkerSymbol.STYLE.CIRCLE);
			    private long all_feature_layer_id2;
			
			
			    @Override
			    protected void onCreate(Bundle savedInstanceState) {
			        super.onCreate(savedInstanceState);
			        setContentView(R.layout.activity_zone_map);
			        ButterKnife.bind(this);
			
			        String x = UserInformationManager.getInstance().getUserCurrentVillageLocationX();
			        String y = UserInformationManager.getInstance().getUserCurrentVillageLocationY();
			
			        if((x != "") && (y != ""))
			        {
			            default_x = Double.parseDouble(x);
			            default_y = Double.parseDouble(y);
			
			            //mapView.centerAt(default_x,default_y,true);
			        }
			
			      
			
			    }
			 
			
			
			
			    private void addFeature2Map(List<CommuniteAllLookResponseBody.ResultBean.FeatureListBean> featureList)
			    {
			        for (CommuniteAllLookResponseBody.ResultBean.FeatureListBean newFeature : featureList) {
			
			            Boolean add_text_g = false;
			            SimpleMarkerSymbol symbol = mGreenMarkerSymbol;
			
			            Point pt = new Point(newFeature.getFeatureX(), newFeature.getFeatureY());
			
			            RobotoCondensed-Regular.ttf");//设置字体
			
			   
			            double[] xxx = CoordinateFunction.lonLat2Mercator(pt.getX(), pt.getY());
			
			            Point mapPoint = new Point(xxx[0], xxx[1]);
			
			            HashMap attr = new HashMap();
			            attr.put("id", newFeature.getFeatureId());
			            attr.put("name", newFeature.getFeatureName());
			            attr.put("x", newFeature.getFeatureX());
			 
			
			            PictureMarkerSymbol pictureMarkerSymbol=null;
			            Bitmap mBitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.map_layer_1_circle);
			            Drawable drawable = new BitmapDrawable(null, mBitmap);
			            pictureMarkerSymbol = new PictureMarkerSymbol(drawable);
			
			            Graphic graphic2 = new Graphic(mapPoint, pictureMarkerSymbol, attr);
			            GraphicsLayer tmp2 = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id2);
			            tmp2.addGraphic(graphic2);
			
			
			            Graphic graphic = new Graphic(mapPoint, mTestRedMarkerSymbol);
			            GraphicsLayer tmp = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id);
			            tmp.addGraphic(graphic);
			
			
			
			        }
			    }
			
			    private void initMapView() {
			        // 设置授权信息（目的：去除水印）
			        ArcGISRuntime.setClientId("1G3512J1UJmwHvHq");
			
			        double[] xxx = CoordinateFunction.lonLat2Mercator(default_x, default_y);
			        Point mapPoint = new Point(xxx[0], xxx[1]);
			        Envelope evn = new Envelope(mapPoint, 2000, 2000);
			        mapView.setExtent(evn);
			
			        String tile_url = "http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=7";
			        //String tile_url = "http://192.168.74.0:8080/MapServer/Service?server=MapTile&map=default"
			        // Add a MBTilesLayer on top with 50% opacity
			        ZTTiledLayer mbLayer = new ZTTiledLayer(tile_url);
			        //mbLayer.setOpacity(0.5f);
			        mapView.addLayer(mbLayer);
			        // enable map to wrap around
			        mapView.enableWrapAround(true);
			
			        drawCustomIcon();
			
			        drawDefaultIcon();
			
			
			
			
			        GraphicsLayer new_house_layer_2 = new GraphicsLayer();
			        mapView.addLayer(new_house_layer_2);
			        long new_house_layer_2_id = new_house_layer_2.getID();
			
			        GraphicsLayer new_house_layer_3 = new GraphicsLayer();
			        mapView.addLayer(new_house_layer_3);
			        long new_house_layer_3_id = new_house_layer_3.getID();
			
			        Log.d("location_layer_id", String.format("%d", new_house_layer_3_id));
			        Log.d("new_house_layer_id", String.format("%d", new_house_layer_3_id));
			
			
			        mapView.setOnSingleTapListener(onSingleTapListener);
			//        mapView.setOnZoomListener(onZoomListener);
			
			
			
			    }
			
			    /**
			     * 绘制系统自带的图标
			     */
			    private void drawDefaultIcon() {
			        GraphicsLayer all_feature_layer = new GraphicsLayer();
			        mapView.addLayer(all_feature_layer);
			        all_feature_layer_id = all_feature_layer.getID();
			    }
			
			    OnSingleTapListener onSingleTapListener = new OnSingleTapListener() {
			        @Override
			        public void onSingleTap(float v, float v1) {
			            Point pt = mapView.toMapPoint(v, v1);
			
			            GraphicsLayer tmp_layer = (GraphicsLayer) mapView.getLayerByID(all_feature_layer_id2);
			            int[] ids = tmp_layer.getGraphicIDs(v, v1, 2);
			
			            for (int id : ids) {
			                Graphic tmp = tmp_layer.getGraphic(id);
			
			
			                String feature_id = tmp.getAttributeValue("id").toString();
			
			
			                double y = Double.parseDouble(tmp.getAttributeValue("y").toString());
			                double x = Double.parseDouble(tmp.getAttributeValue("x").toString());
			
			                double[] xxx = CoordinateFunction.lonLat2Mercator(x, y);
			
			                Point mapPoint = new Point(xxx[0], xxx[1]);
			
			                mapView.zoomTo(mapPoint, 4);
			
			                ToastUtils.showToastShort(tmp.getAttributeValue("name").toString());
			                break;
			
			
			            }
		
			        }
			    };
			
			    /**
			     * 自定义图片显示点
			     */
			    private void drawCustomIcon() {
			        GraphicsLayer all_feature_layer2 = new GraphicsLayer();
			        //设置透明度
			        all_feature_layer2.setOpacity(50);
			        mapView.addLayer(all_feature_layer2);
			        all_feature_layer_id2 = all_feature_layer2.getID();
			    }
			}





## 地图绘线 ##



* GraphicsLayer：继承自Layer，是图形数据集中展现的地方。GraphicsLayer是保留在内存中的一种图层（与 ArcMap、ArcGIS Engine、ArcGIS Server ADF程序中的相应概念类似），顾名思义是很多Graphic的集合，而所有与用户交互的内容通常都用Graphic来显示。比如多边形查询中用户画出的 多边形，属性/空间查询结果中的所有要素（Feature）等内容，都是Graphic。可以说，除了地图本身，基本上看到的所有与地理位置有关的东西都 可以用Graphic来表示。Graphic对象有3个重要的属性：Geometry，Symbol和Attributes。Geometry代表了一个 Graphic的几何形状（可疑是点、线、面任意一种）或地理位置，而Symbol则表示Graphic的呈现样子，比如颜色、效果，同时有了这两个属性 （缺一不可），Graphic就可以显示到地图上了。而Attributes是键值对集合，可在里面存储任意类型的对象，比如一个要素的属性信息。

* （10）FeatureLayer：继承自GraphicsLayer，这也是ArcGIS API for Silverlight/WPF 中的亮点之一，通过它可以完成一些比较炫的功能。FeatureLayer继承自GraphicsLayer，它与后者的区别 是，GraphicsLayer中的Graphic都是人为创建出来的，而FeatureLayer中的Graphic都是从ArcGIS Server发布的服务中读取出来的，因此FeatureLayer比GraphicsLayer多了一个URL属性。这个URL通常指向一个 ArcGIS Server发布的MapService或FeatureService的子图层（对应一个FeatureClass）。FeatureLayer有了这 个URL后，就可以读取出该服务对应子图层里的所有要素内容，因此FeatureLayer里Graphic的Geometry属性会自动被 FeatureClass的Shape字段填充，而Graphic的Attributes字段则会根据要求，被FeatureClass中的属性信息所填 充。如果发布服务的服务器是ArcGIS Server 10版本，则Graphic的Symbol属性会自动被服务的DrawingInfo信息填充。另外，FeatureLayer是客户端API中对 FeatureService的唯一载体，这是它另一个非常重要的作用（也是主要作用）。

* （11）ElementLayer：继承自Layer，它可以用来专门呈现Silverlight/WPF中原生的 FrameworkElement，比如视频，音频等。虽然在FillSymbol的Fill属性中也能利用Brush类来展现一段视频，但毕竟有些“小 气”，在ElementLayer中可以大大方方的放置Silverlight/WPF元素。虽然在Map控件之外，Grid等布局元素中也能放置 Silverlight的东西，但是ElementLayer的优势在于：当一些非地理数据元素随着地图范围的变化（放大，缩小，平移）的时候，无须在地 图Extent变化后重新计算客户端坐标，再改变这些元素的位置。