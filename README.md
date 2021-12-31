**开发工具：Android studio 3.5.2**

**使用到的第三方库：calendarview**

**使用到的轻量级存储：sharedPreferences/sqlite**

### 1.1 初始页面

layout：activity_main.xml

java：MainActivity

页面效果：

<img src="README.assets/image-20211230205352365.png" alt="image-20211230205352365" style="zoom:25%;" />

```
<LinearLayout><!--设置orientation=vertical，垂直布局-->
	<ImageView/>
	<!--设置layout_gravity="center"装中间的桃子；
	onclick="period"添加了点击事件，点击后执行period方法，跳转到主页面-->
	<TextView/><!--gravity="bottom"文字在桃子下面-->
</LinearLayout>
```

### 1.2 主页面

**布局结构**

布局文件：activity_period.xml

java文件：PeriodActivity.java

<img src="README.assets/image-20211230210602893.png" alt="image-20211230210602893" style="zoom:50%;" />

####1.2.1 Toolbar导航栏

**========================Toolbar导航栏=========================**

```
<LinearLayout><!--垂直布局-->
	<!--导航栏-->
	<androidx.appcompat.widget.Toolbar>
	<!--navigationIcon为左侧返回的图标，点击后返回到初始页面-->
		<RelativeLayout>
			<TextView/><!--“经期日历”四个字显示的地方-->
			<ImageView/>
			<!--右侧小日历图标，id:weekcalendar，点击后转换成小日历视图，			底下的年月会隐藏，通过visibility=GONE属性隐藏-->
			<TextView><!--点击日历的某一天会显示月和日-->
		</RelativeLayout>
		<!--使用relativeLayout的原因：设置布局更灵活-->
	</androidx.appcompat.widget.Toolbar>
</LinearLayout>
```

**1.点击小日历图标转换成小日历视图功能实现**

​	（1）在PeriodActivity.java文件中，用ImageView类型的 mTextMonthDay用findViewById找到图标weekcalendar（line:120）

​	（2）添加onclick监听器（line:208）

​			如果日历不是展开的也就是说如果目前不是小日历模式，那么则转换成小日历模式

```
mcalendarLayout.expand();//转换成小日历模式
mCalendarView.showYearSelectLayout(mYear);//获取当前年份所有月并显示
mTextMonthDay1.setText(String.valueOf(mYear));//显示年份的地方
tvMonth.setVisibility(View.GONE);//显示年月的地方隐藏
```

<img src="README.assets/image-20211230212639913.png" alt="image-20211230212639913" style="zoom:25%;" />

#### 1.2.2 显示当前年月的TextView

id:tv_month

在PeriodActivity中tvMonth绑定控件tv_month，用对应方法获取当前年当前月（line:272）

```
int y=mCalendarView.getCurYear();//获取年
int m=mCalendarView.getCurMonth();//获取月
tvMonth.setText( y+"年"+m+"月" );
```

#### 1.2.3 日历显示

**布局结构：**

```
<com.haibin.calendarview.CalendarLayout>
<!--第三方开源库calendarview的布局-->
	<com.haibin.calendarview.CalendarView/>
	<!--显示日历的地方;id=calendarView
	month_view=
	"com.example.periodrecords2.CustomMultiMonthView"-->
	<!--加载CustomMultiMonthView文件，用该文件实现UI的绘制-->
</com.haibin.calendarview.CalendarLayout>
```

在java文件中，用一个Calendarview类型的mCalendarView绑定日历，因为用到了日历多选，所以在mCalendarView需要绑定第三方开源库中的多选事件监听器和视图转换监听器以及选中事件监听器/月份转换（line：222），并给日历限制了范围为过去三年到当前下一个月，依然是calendarview库里的方法（line：277）

**===========================给日期分类==========================**

**方法：setDatas(line:305)**

将日期分为六类：安全期/易孕期/排卵期/预测经期/经期/今天

不同的类别在日历上的绘制样式不同

**onCalendarMultiSelect方法(line:450)**

选中某一天后会在日历上绘制成红色，用getYear（）等方法获取选中的日期存入sqlite中

**sqlite**

包含三个文件：CRDB(实现数据库的增、删)

​							dateclass（定义了一个类，用来规定输入的数据的格式）

​							Datedb（建数据库）

**=========================绘制日历=============================**

CustomMultiMonthView.java文件

在onDrawSelected方法中（line:43），多选的绘制样式，如果中间断开了，那么连接口会变成方的，如果没断则是圆的

****

#### 1.2.4 添加注意事项

![image-20211230221050684](README.assets/image-20211230221050684.png)

布局：

```

```

![image-20211230224102188](README.assets/image-20211230224102188.png)