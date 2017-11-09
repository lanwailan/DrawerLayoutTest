# DrawerLayoutTest
Toolbar + DrawerLayout
2.添加Toolbar

由于Toolbar是继承自View，所以可以像其他标准控件一样直接主布局文件添加Toolbar，但是为了提高Toolbar的重用效率，可以在layout创建一个custom_toolbar.xml代码如下：

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
说明：

android.support.v7.widget.Toolbar - 当然如果只在Lollipop中可以直接使用Toolbar而不需要加上v7支持
xmlns:app - 自定义xml命名控件，在AS中可以直接指定res-auto而不需要使用完整包名
android:background 和 android:minHeight 均可以在styles.xml中声明
2.添加DrawerLayout

和Toolbar类似，为了提高代码重用效率，可以在layout中创建一个custom_drawerlayout.xml代码如下:

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
Drawerlayout标签中有两个子节点，一个是左边菜单，一个是主布局，另外需要在左边菜单起始位置设置为android:layout_gravity="start"

3.实现activity_main.xml

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
直接使用include标签，简洁明了
