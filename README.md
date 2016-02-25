BottomSheets
=======================
Android Support Library 23.2里的 Design Support Library新加了一个Bottom Sheets控件，一个底部表，就是我们经常在分享或者地图、音乐等app看到的效果

 <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3VW9HUDhtTE5XZVk/components_bottomsheets_usage2.png" width="360" height="640" />
 <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3dDZKN1lHNG1TekU/components_bottomsheets_usage1.png" width="360" height="640" />

昨天晚上Support Library 23.2包还没能更新，官方视频就已经透露出了，[视频地址](https://youtu.be/7E2lNBM38IE?list=PLWz5rJ2EKKc9e0d55YHgJFHXNZbGHEXJX),从视频看出Support Library 23.2包新增了:

* 1. Support Vector Drawables and Animated Vector Drawables
* 2. AppCompat DayNight theme
* 3. Design Support Library: Bottom Sheets
* 4. Support v4: MediaBrowserServiceCompat
* 5. Custom Tabs

具体可以上官网博客看看。

这篇文章我给大家说说BottomSheetBehavior的使用及注意的地方

##Usage
###(1)在布局文件xml中
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/cl"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    >

    <android.support.v4.widget.NestedScrollView
        android:id="@+id/bottom_sheet"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:behavior_hideable="true"
        app:behavior_peekHeight="50dp"
        app:layout_behavior="@string/bottom_sheet_behavior"
        >
          <!-- NestedScrollView里设置你的底部表长什么样的-->
    </android.support.v4.widget.NestedScrollView>
</android.support.design.widget.CoordinatorLayout>

```
###(2)在java代码中
```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ns);

        // The View with the BottomSheetBehavior
        CoordinatorLayout coordinatorLayout = (CoordinatorLayout) findViewById(R.id.cl);
        View bottomSheet = coordinatorLayout.findViewById(R.id.bottom_sheet);
        final BottomSheetBehavior behavior = BottomSheetBehavior.from(bottomSheet);
        behavior.setBottomSheetCallback(new BottomSheetBehavior.BottomSheetCallback() {
            @Override
            public void onStateChanged(@NonNull View bottomSheet, int newState) {
                //这里是bottomSheet 状态的改变，根据slideOffset可以做一些动画
            }

            @Override
            public void onSlide(@NonNull View bottomSheet, float slideOffset) {
                //这里是拖拽中的回调，根据slideOffset可以做一些动画
            }
        });
    }
```

其实也挺简单的，我来解释一下。通过附加一个BottomSheetBehavior 给CoordinatorLayout的子视图，上文xml中的是NestedScrollView(adding app:layout_behavior = " android.support.design.widget.BottomSheetBehavior”)，当然，RecyclerView也是可以的。现在你肯定有疑问了，像listView、ScrollView 这些可以吗？官方说API 21+就可以。

```xml
     app:behavior_hideable="true"
     app:behavior_peekHeight="50dp"
```
这两个属性我说说，peekHeight是当Bottom Sheets关闭的时候，底部下表我们能看到的高度，hideable 是当我们拖拽下拉的时候，bottom sheet是否能全部隐藏。
如果你需要监听Bottom Sheets回调的状态，可以通过setBottomSheetCallback来实现，onSlide方法是拖拽中的回调，根据slideOffset可以做一些动画
onStateChanged方法可以监听到状态的改变,总共有5种

* STATE_COLLAPSED: 关闭Bottom Sheets,显示peekHeight的高度，默认是0
* STATE_DRAGGING:  用户拖拽Bottom Sheets时的状态
* STATE_SETTLING: 当Bottom Sheets view释放的状态
* STATE_EXPANDED: 当Bottom Sheets view展开放的状态
* STATE_HIDDEN: 当Bottom Sheets view隐藏的状态

我也简单的写了两个demo，你可以看我源码是怎么用的

![](https://github.com/android-cjj/BottomSheets/blob/master/gif%2Fbs1.gif)
![](https://github.com/android-cjj/BottomSheets/blob/master/gif%2Fbs12.gif)

使用就这些了，接下来我们来讲讲改注意的地方，应该说怎样更好的使用它。

###（1）关闭Bottom Sheets的行为
可以通过下图的形式，拖拽、点击bottom sheet之外的地方和通过‘x’按钮

 <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3a3Z2UGZJMkRWamM/components_bottomsheets_behavior1.png" width="240" height="240" />
  <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3cGt6TlA0ZzNLdDg/components_bottomsheets_behavior2.png" width="240" height="240" />
   <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3cTdiQWp2TkI0NEE/components_bottomsheets_behavior3.png" width="240" height="240" />
   
###（2）合适的视图设计

 <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3MC0zbFNZaU1lUkk/components_bottomsheets_modal9.png" width="360" height="640" />
  <img src="https://material-design.storage.googleapis.com/publish/material_v_4/material_ext_publish/0Bzhp5Z4wHba3TnJEQUF3Y0JVb2c/components_bottomsheets_modal10.png" width="360" height="640" />






















