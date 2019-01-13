# 安卓轻量级底部导航栏

目前安卓开发中常常会用到底部导航栏这个控件，但是自己从零开始做一个又太麻烦。因此，我封装了一个底部导航栏，同时，也做了一些修改，用于顶部也十分合适。下面是示例图：



## 使用方法：

### 1.添加依赖

首先，在build.gradle文件下加入 maven {url 'https://jitpack.io'}

```javascript
allprojects {
	repositories {
		google()
		jcenter()
		maven {url "https://jitpack.io"}
	}
}
```

然后在dependencies下加入依赖

```js
implementation 'com.github.EHENJOOM:BottomBar:1.0.0'
```

### 2.在布局文件中添加frameLayout和导航栏

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <FrameLayout
        android:id="@+id/fragment_container"
        android:layout_above="@+id/bottombar"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.example.ehenjoom.bottombar.BottomBar
        android:id="@+id/bottombar"
        android:layout_width="match_parent"
        android:layout_height="46dp"
        android:layout_alignParentBottom="true" />
</RelativeLayout>
```

FrameLayout是用来显示fragment内容的。

### 3.在java代码中添加导航栏item,同时建立各个item对应的类

```java
BottomBar bottomBar=findViewById(R.id.bottombar);
bottombar.setContainer(R.id.fragment_container)
    .addItem(Home.class,"首页",homeicon_before,homeicon_after)
    .addItem(Message.class,"消息",messageicon_before,messageicon_after)
    .addItem(Me.class,"我",meicon_before,meicon_after)
    .create();
```

在java代码里首先要调用setContainer()方法设置farmeLayout，然后添加导航栏的item，然后调用设置属性的各个api，最后一定要调用create()方法创建。

##### 注意：此Activity要继承AppCompatActivity才能运行，否则程序会崩溃。关于这点，后续会更新版本来支持其他Activity。

##### 另：如果需要设置成没有图标的导航栏，只需把icon的宽高设置为0即可。

### 4.在item对应的类文件里设置布局

由于使用的是frameLayout，因此item对应的类文件里不能继承Activity，要继承Fragment才行。

```java
public class Home extends Fragment {
    
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState){
        View view=inflater.inflate(R.layout.homelayout_fragment,container,false);
        // 如果需要实例化控件，在这里实例化。
        TextView textView=view.findViewById(R.id.textView);
        textView.setText("首页");
        return view;
    }
    
    @Override
    public void onActivityCreated(Bundle savedInstanceState){
        super.onActivityCreated(savedInstanceState);
        // Fragment里的控件监听事件在这里面实现
    }
}
```

### 5.设置BottomBar的属性

#### 在java代码里设置属性

|               api名称                |                api作用                 |
| :----------------------------------: | :------------------------------------: |
| setTitleBeforeAndAfterColor(int,int) |         设置文字选中前后的颜色         |
|          setTitleSize(int)           |       设置文字大小(默认dp为单位)       |
|          setIconWidth(int)           |             设置图标的宽度             |
|          setIconHeight(int)          |              设置图标的高              |
|       setTitleIconMargin(int)        |          设置文字与图标的间隙          |
|         setFirstChecked(int)         |      设置默认选中的item(默认为0)       |
|     isShowAboveBoundary(boolean)     |    设置是否显示上方分界线(默认显示)    |
|    isShowBottomBoundary(boolean)     |   设置是否显示下方分界线(默认不显示)   |
|       isShowAboveClue(boolean)       | 设置是否显示上方选中提示线(默认不显示) |
|      isShowBottomClue(boolean)       |  设置是否显示下方选中提示线(默认显示)  |
|       setAboveClueHeight(int)        |          设置上方提示线的粗细          |
|       setBottomClueHeight(int)       |          设置下方提示线的粗细          |
|        setBoundaryColor(int)         |       设置分界线的颜色(默认黑色)       |

java代码：

```java
bottombar.setTitleSize(14)
    .setFirstChecked(2)
    .isShowAboveClue(true)
```

#### 或者在xml标签里设置属性

要使用这些属性，首先要加入命名空间

```xml
xmlns:app="http://schemas.android.com/apk/res-auto"
```

|        标签名        |              对应属性              |
| :------------------: | :--------------------------------: |
|   titleBeforeColor   |          文字选中前的颜色          |
|   titleAfterColor    |          文字选中后的颜色          |
|      titleSize       |       文字大小(默认dp为单位)       |
|      iconWidth       |             图标的宽度             |
|      iconHeight      |            设置图标的高            |
|   titleIconMargin    |          文字与图标的间隙          |
|     firstChecked     |           默认选中的item           |
| isShowAboveBoundary  |    是否显示上方分界线(默认显示)    |
| isShowBottomBoundary |   是否显示下方分界线(默认不显示)   |
|   isShowAboveClue    | 是否显示上方选中提示线(默认不显示) |
|   isShowBottomClue   |  是否显示下方选中提示线(默认显示)  |
|   aboveClueHeight    |          上方提示线的粗细          |
|   bottomClueHeight   |          下方提示线的粗细          |
|    boundaryColor     |       分界线的颜色(默认黑色)       |

布局文件：

```xml
<com.github.EHENJOOM.BottomBar
       xmlns:app="http://schemas.android.com/apk/res-auto"
       android:id="@+id/bottombar"
       android:layout_width="match_parent"
       android:layout_height="45dp"
                               
       app:titleSize="14"
       app:isShowAboveClue="true"
       app:aboveClueHeight="6"/>
```

##### 另：item选中时的提示线会根据文字长度自动适配。
