# DrawerLayoutTest
Toolbar + DrawerLayout

Demo View
http://www.jcodecraeer.com/uploads/20150303/1425353302511647.gif

1.添加Toolbar

由于Toolbar是继承自View，所以可以像其他标准控件一样直接主布局文件添加Toolbar，但是为了提高Toolbar的重用效率，可以在layout创建一个custom_toolbar.xml代码如下：
```java
<?xml version="1.0" encoding="utf-8"?>
    <android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/tl_custom"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        android:minHeight="?attr/actionBarSize"
        android:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        app:theme="@style/ThemeOverlay.AppCompat.ActionBar">
</android.support.v7.widget.Toolbar>
```

说明：

android.support.v7.widget.Toolbar - 当然如果只在Lollipop中可以直接使用Toolbar而不需要加上v7支持
xmlns:app - 自定义xml命名控件，在AS中可以直接指定res-auto而不需要使用完整包名
android:background 和 android:minHeight 均可以在styles.xml中声明

2.添加DrawerLayout

和Toolbar类似，为了提高代码重用效率，可以在layout中创建一个custom_drawerlayout.xml代码如下:
```java
<?xml version="1.0" encoding="utf-8"?>
    <android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/dl_left"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    <!--主布局-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <ImageView
            android:id="@+id/iv_main"
            android:layout_width="100dp"
            android:layout_height="100dp" />
    </LinearLayout>
    <!--侧滑菜单-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#fff"
        android:layout_gravity="start">
        <ListView
            android:id="@+id/lv_left_menu"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:divider="@null"
            android:text="DrawerLayout" />
    </LinearLayout>
</android.support.v4.widget.DrawerLayout>
```
Drawerlayout标签中有两个子节点，一个是左边菜单，一个是主布局，另外需要在左边菜单起始位置设置为android:layout_gravity="start"

3.实现activity_main.xml
```java
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
        <!--Toolbar-->
        <include layout="@layout/custom_toolbar" />
        <!--DrawerLayout-->
        <include layout="@layout/custom_drawerlayout" />
</LinearLayout>
```
直接使用include标签，简洁明了

4.完善Java代码
```java
public class MainActivity extends ActionBarActivity {
    //声明相关变量
    private Toolbar toolbar;
    private DrawerLayout mDrawerLayout;
    private ActionBarDrawerToggle mDrawerToggle;
    private ListView lvLeftMenu;
    private String[] lvs = {"List Item 01", "List Item 02", "List Item 03", "List Item 04"};
    private ArrayAdapter arrayAdapter;
    private ImageView ivRunningMan;
    private AnimationDrawable mAnimationDrawable;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        findViews(); //获取控件
        //京东RunningMan动画效果，和本次Toolbar无关
        //mAnimationDrawable = (AnimationDrawable) ivRunningMan.getBackground();
        //mAnimationDrawable.start();
        toolbar.setTitle("Toolbar");//设置Toolbar标题
        toolbar.setTitleTextColor(Color.parseColor("#ffffff")); //设置标题颜色
        setSupportActionBar(toolbar);
        getSupportActionBar().setHomeButtonEnabled(true); //设置返回键可用
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        //创建返回键，并实现打开关/闭监听
        mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout, toolbar, R.string.open, R.string.close) {
            @Override
            public void onDrawerOpened(View drawerView) {
                super.onDrawerOpened(drawerView);
                //mAnimationDrawable.stop();
            }
            @Override
            public void onDrawerClosed(View drawerView) {
                super.onDrawerClosed(drawerView);
                //mAnimationDrawable.start();
            }
        };
        mDrawerToggle.syncState();
        mDrawerLayout.addDrawerListener(mDrawerToggle);
        //设置菜单列表
        arrayAdapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, lvs);
        lvLeftMenu.setAdapter(arrayAdapter);
    }
    private void findViews() {
        ivRunningMan = (ImageView) findViewById(R.id.iv_main);
        toolbar = (Toolbar) findViewById(R.id.tl_custom);
        mDrawerLayout = (DrawerLayout) findViewById(R.id.dl_left);
        lvLeftMenu = (ListView) findViewById(R.id.lv_left_menu);
    }
}
```

5.当然比较重要还有styles.xml和colors.xml，具体如下
```java
<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!--状态栏颜色-->
        <item name="colorPrimaryDark">@color/Indigo_colorPrimaryDark</item>
        <!--Toolbar颜色-->
        <item name="colorPrimary">@color/Indigo_colorPrimary</item>
        <!--返回键样式-->
        <item name="drawerArrowStyle">@style/AppTheme.DrawerArrowToggle</item>
    </style>
    <style name="AppTheme.DrawerArrowToggle" parent="Base.Widget.AppCompat.DrawerArrowToggle">
        <item name="color">@android:color/white</item>
    </style>
</resources>

<resources>
    <color name="Indigo_colorPrimaryDark">#303f9f</color>
    <color name="Indigo_colorPrimary">#3f51b5</color>
    <color name="Indigo_nav_color">#4675FF</color>
</resources>
```
