# Android开发之Dialog的使用 #
# dialog的使用 #

##  dialog.setCancelable与setCanceledOnTouchOutside的区别 ##


	dialog.setCancelable(false);
	dialog弹出后会点击屏幕或物理返回键，dialog不消失
	
	dialog.setCanceledOnTouchOutside(false);
	dialog弹出后会点击屏幕，dialog不消失；点击物理返回键dialog消失
	 







## 2 button的使用  ##
### 设置button不可已被点击 ###
	
 	btn.setClickable(false);





## 3. gridview 的使用 ##

#### 1. 怎样让GridView不可以滑动 ####

* 可以使用自定义的方式来完成对应的需求
* 1.定义一个固定大小和不可滑动的GridView
* 代码如下



		public class NoScrollGridView extends GridView {
		    public NoScrollGridView(Context context) {
		        super(context);
		    }
		    public NoScrollGridView(Context context, AttributeSet attrs) {
		        super(context, attrs);
		    }
		    public NoScrollGridView(Context context, AttributeSet attrs, int  defStyle) {
		        super(context, attrs, defStyle);
		    }
		
		    public boolean dispatchTouchEvent(MotionEvent ev) {
		        if (ev.getAction() == MotionEvent.ACTION_MOVE) {
		            return true;  //禁止GridView滑动
		        }
		
		        return super.dispatchTouchEvent(ev);
		    }
		}


* 2.上面只是提供了一种简单的思路，当然我们的日常开发中还存在着另一种情况，就是我们在GridView中显示图片，这时候我们是希望所有的图片全部显示，这时候就应当根据GridView中的内容的实际的大小去测量和摆放对应的控件

		
		public class NoScrollAndShowAllGridView extends GridView {
		    public NoScrollAndShowAllGridView(Context context) {
		        super(context);
		    }
		    public NoScrollAndShowAllGridView(Context context, AttributeSet attrs) {
		        super(context, attrs);
		    }
		    public NoScrollAndShowAllGridView(Context context, AttributeSet attrs, int  defStyle) {
		        super(context, attrs, defStyle);
		    }
		
		    public boolean dispatchTouchEvent(MotionEvent ev) {
		        if (ev.getAction() == MotionEvent.ACTION_MOVE) {
		            return true;  //禁止GridView滑动
		        }
		
		        return super.dispatchTouchEvent(ev);
		    }
		
		    @Override
		    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		        // TODO 自动生成的方法存根
		        int expandSpec = MeasureSpec.makeMeasureSpec(
		                Integer.MAX_VALUE >> 2, MeasureSpec.AT_MOST);
		        super.onMeasure(widthMeasureSpec, expandSpec);
		    }
		}

* 这里的  onMeasure 方法会在控件中的内容发生变化的时候进行调用