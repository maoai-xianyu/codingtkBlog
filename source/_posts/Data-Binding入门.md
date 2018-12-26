---
title: Data Binding入门
date: 2018-12-05 19:18:27
tags:
	- Android
	- Data Binding
	- MVVM
categories: "Android Data Binding"
---

## Data Binding
讲解 用于Android MVVM 的学习，用以丰富自己的知识库。

1. MVVM 
![image](http://note.youdao.com/yws/public/resource/3cbd34e35d942c29230fdbfa8a53051a/A1B734F7FD1148099B1380A718C1C5FA?ynotemdtimestamp=1545734131336)
2. 提高开发效率
3. 性能高/功能强

<!--more-->

用途

1. 去掉Activities & Fragments内的UI代码
2. XML变成UI的唯一真实来源
3. 减少定义View id 的主要用途

类似方案

1. ButterKnife
2. Andriod Annotations
3. RoboBinding

优势

1. 去除Activity/Fragment中的U代码
2. 性能超过手写代码，安全（不会id错而crash）
3. 保证执行在主线程

主要劣势

1. IDE支持还不那么完善
2. 报错信息不那么直接
3. 没有重构支持


## 使用Data Binding
```
app Module
android {
    compileSdkVersion rootProject.ext.compileSdkVersion
    ...
    dataBinding{
        enabled = true
    }
}
```

### Layout文件改写
在源layout文件外套一层标签

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/c_FFFFFF">

    
    </RelativeLayout>
</layout>
```

### 除去findviewbyID

1. No more findviewbyID
2. Binding.xxxView
3. 生成规则

```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <TextView
        android:id="@+id/tv_first_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <TextView
        android:id="@+id/tv_last_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</layout>
                
Employee employee = new Employee("Tom", "Jack");

AtyDataBindingBinding binding = DataBindingUtil.setContentView(this, R.layout.aty_data_binding);
binding.tvFirstName.setText(employee.getFirstName());
binding.tvLastName.setText(employee.getLastName());

aty_data_binding 对应的是  AtyDataBindingBinding
tv_first_name  对应的是 tvFirstName

```
### UI/事件绑定

1. Bind UI  setXXX  setVariable

```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="employee"
            type="com.mao.cn.learnDevelopProject.model.Employee" />

    </data>

    <LinearLayout
        android:id="@+id/rl_content"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/rl_header"
        android:orientation="vertical">

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="输入 first name" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="输入 last name" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{employee.firstName}" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{employee.lastName}" />
    </LinearLayout>

</layout>

 AtyDataBindingBinding binding = DataBindingUtil.setContentView(this, R.layout.aty_data_binding);
binding.setEmployee(employee);
// 两种方式都可以
//binding.setVariable(com.mao.cn.learnDevelopProject.BR.employee,mEmployee);
```

2. 事件

    1. andriod:onClick
    2. android:onLongClick
    3. android:onTextChanged
    4. ...
    5. 方法引用
    6. 监听器绑定

```
<variable
    name="presenter"
    type="com.mao.cn.learnDevelopProject.ui.activity.DataBindingActivity.Presenter"/>
<EditText
    android:id="@+id/tab"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:onTextChanged="@{presenter.onTextChanged}"
    android:text="输入 first name" />   
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="@{presenter.onclick}"
    android:text="@{employee.firstName}" />
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="@{()->presenter.onClickListenerBinding(employee)}"
    android:text="@{employee.lastName}" />


public class DataBindingActivity extends RxAppCompatActivity {

    private Employee mEmployee = new Employee("Tom", "Jack");
    AtyDataBindingBinding binding;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = DataBindingUtil.setContentView(this, R.layout.aty_data_binding);
        binding.setEmployee(mEmployee);
        binding.setPresenter(new Presenter());
        //binding.setVariable(BR.employee, mEmployee);
    }


    public class Presenter {
        // 方法引用
        public void onTextChanged(CharSequence text, int start, int lengthBefore, int lengthAfter) {
            mEmployee.setFirstName(text.toString());
            binding.setEmployee(mEmployee);
        }
        //  方法引用
        public void onClick(View view){
            ToastUtils.show("点击了");
        }
        //监听器绑定
        public void onClickListenerBinding(Employee employee){
            ToastUtils.show("点击了"+employee.getLastName());
        }
    }
}
    
```

### Data Binding 原理

1. android.binding
2. BR
3. XxxBinding

开始编译-->处理layout文件-->解析表达式-->java编译-->解析依赖-->找到setter

性能:

1. 0反射
2. findViewById需要遍历整个viewgroup,现在只需做一次
3. 使用位标记来检验更新
4. 数据改变在下一次批量更新才会触发操作
5. 缓存表达式，如：

```
a?(b?c:d):e
f?(b?c:d):f
```
### 表达式
1. 二元 &|^
2. 一元 =-!~/
3. 移位 >> <<  >>>
4. 比较 == > < >= <=
5. Instanceof
6. Grouping()
7. 文字 character,String,numeric,null
8. Cast 类型转换
9. 方法调用

```
<EditText
    android:id="@+id/tab"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:onTextChanged="@{presenter::onTextChanged}"
    android:text="输入 first name" />   
```

10. Feild访问
11. Array 访问[]
12. 三元 ?:

```
<ata>
    <import type="android.util.SparseArray"/>
    <import type="android.view.View" />
    <import type="java.util.Map"/>
    <import type="java.util.List"/>
    <variable name="list" type="List<String>"/>
    <variable name="sparse" type="SparseArray<String>"/>
    <variable name="map" type="Map<String, String>"/>
    <variable name="index" type="int"/>
    <variable name="key" type="String"/>
</data>
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="@{()->presenter.onClickListenerBinding(employee)}"
    android:text="@{employee.lastName}"
    android:visibility="@{employee.goodGuy? View.VISIBLE:View.GONE}" />
```

### 表达式-缺省

1. this
2. super
3. new
4. 显示泛型调用 Explicit generic invocation

### null 合并运算符 ??
取非空表达式

```
null 合并运算符：??  等同于  ?:
android:text=“@{user.displayName ?? user.lastName}”  
android:text=“@{user.displayName != null ? user.displayName : user.lastName}” 
```
### 表达式eg

Margin @dimen+@dimen

```
android:text="@{String.valueOf(index + 1)}"
android:visibility="@{age < 13 ? View.GONE : View.VISIBLE}"
android:transitionName='@{"image_" + id}'
```
### 避免空指针

1. 自动空指针检查
Data Binding 代码生成时会自动检查是否为 null，来避免出现 null pointer exception 错误，
```
例如在表达式 @{user.name} 中，
如果 user 是 null，user.name 会赋予它的默认值 null，
如果你引用 @{user.age}，age 是 int 类型，那么它的默认值是 0
```
2. 数据越界 需要自己注意

### include
1. bind

```
<?xml version=“1.0” encoding=“utf-8”?>  
<layout xmlns:android=“http://schemas.android.com/apk/res/android”  
        xmlns:bind=“http://schemas.android.com/apk/res-auto”>  
   <data>  
       <variable name=“user” type=“com.example.User”/>  
   </data>  
   <LinearLayout  
       android:orientation=“vertical”  
       android:layout_width=“match_parent”  
       android:layout_height=“match_parent”>  
       <include layout=“@layout/name”  
           bind:user=“@{user}”/>  
       <include layout=“@layout/contact”  
           bind:user=“@{user}”/>  
   </LinearLayout>  
</layout>
在这里，必须有两 name.xml 和 contact.xml 布局文件的用户变量
```

2. 尚不支持direct child 如 root 未 merge

```
据绑定不支持包括合并元素的直接子项。例如，不支持下列布局：
<?xml version=“1.0” encoding=“utf-8”?>  
<layout xmlns:android=“http://schemas.android.com/apk/res/android”  
        xmlns:bind=“http://schemas.android.com/apk/res-auto”>  
   <data>  
       <variable name=“user” type=“com.example.User”/>  
   </data>  
   <merge>  
       <include layout=“@layout/name”  
           bind:user=“@{user}”/>  
       <include layout=“@layout/contact”  
           bind:user=“@{user}”/>  
   </merge>  
</layout> 
```

### viewstub
ViewStub 相比普通 View 有一些不同。ViewStub 一开始是不可见的，当它们被设置为可见，或者调用 inflate 方法时，ViewStub 会被替换成另外一个布局。

因为 ViewStub 实际上不存在于 View 结构中，binding 类中的类也得移除掉，以便系统回收。因为 binding 类中的 View 都是 final 的，所以Android 提供了一个叫 ViewStubProxy 的类来代替 ViewStub 。开发者可以使用它来操作 ViewStub，获取 ViewStub inflate 时得到的视图。

但 inflate 一个新的布局时，必须为新的布局创建一个 binding。因此， ViewStubProxy 必须监听 ViewStub 的 ViewStub.OnInflateListener，并及时建立 binding。由于 ViewStub 只能有一个 OnInflateListener，你可以将你自己的 listener 设置在 ViewStubProxy 上，在 binding 建立之后， listener 就会被触发。

```
   binding.viewStub.setOnInflateListener(new ViewStub.OnInflateListener() {
            @Override
            public void onInflate(ViewStub viewStub, View view) {
                EmployeeUser mEmployee = new EmployeeUser("Mao", "XianYu");
                ViewstubBinding bind = DataBindingUtil.bind(view);
                assert bind != null;
                bind.setEmployeeUser(mEmployee);
            }
        });
        Objects.requireNonNull(binding.viewStub.getViewStub()).inflate();
```
### 数据对象 (Data Objects)
任何 POJO 对象都能用在 Data Binding 中，但是更改 POJO 并不会同步更新 UI。Data Binding 的强大之处就在于它可以让你的数据拥有更新通知的能力。

有三种不同的动态更新数据的机制：

1. Observable 对象
2. Observable 字段
3. Observable 容器类
当以上的 observable 对象绑定在 UI 上，数据发生变化时，UI 就会同步更新。

* Observable 对象

当一个类实现了 Observable 接口时，Data Binding 会设置一个 listener 在绑定的对象上，以便监听对象字段的变动。

Observable 接口有一个添加/移除 listener 的机制，但通知取决于开发者。为了简化开发，Android 原生提供了一个基类 BaseObservable 来实现 listener 注册机制。这个类也实现了字段变动的通知，只需要在 getter 上使用 Bindable 注解，并在 setter 中通知更新即可。

```
public class Employee  extends BaseObservable implements Serializable {

    private String firstName;
    private String lastName;
    private boolean isGoodGuy;
    private boolean mIsFired;

    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.isGoodGuy = true;
    }

    public Employee() {
    }

    @Bindable
    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
        notifyPropertyChanged(BR.firstName);
    }

    @Bindable
    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
        notifyPropertyChanged(BR.lastName);
    }

    public boolean isGoodGuy() {
        return isGoodGuy;
    }

    public void setGoodGuy(boolean goodGuy) {
        this.isGoodGuy = goodGuy;

    }

    public boolean ismIsFired() {
        return mIsFired;
    }

    public void setmIsFired(boolean mIsFired) {
        this.mIsFired = mIsFired;
        notifyChange();
    }

    @Override
    public String toString() {
        return "Employee{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                '}';
    }
}

当数据发生变化时需要调用 notifyPropertyChanged(BR.firstName) 通知系统 BR.firstName 这个 entry 的数据已经发生变化以更新UI。

```

* ObservableFields

创建 Observable 类还是需要花费一点时间的，如果想要省时，或者数据类的字段很少的话，可以使用 ObservableField 以及它的派生 ObservableBoolean、
ObservableByte 、ObservableChar、ObservableShort、ObservableInt、ObservableLong、ObservableFloat、ObservableDouble、
ObservableParcelable 。

ObservableFields 是包含 observable 对象的单一字段。原始版本避免了在存取过程中做打包/解包操作。要使用它，在数据类中创建一个 public final 字段：

```

public class Employee extends BaseObservable implements Serializable {
    public ObservableField<String> name = new ObservableField<>();
    
    //构造 添加数据
    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        // set 为赋值  get 为取值
        name.set("Employee");
    }
}

// 布局文件中获取
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{employee.name}"
    android:textColor="@color/c_69C614" />

```

ObservableBoolean

```
public class Employee extends BaseObservable implements Serializable {
    public ObservableBoolean isFired = new ObservableBoolean();
    
    //构造 添加数据
    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        // set 为赋值  get 为取值
        isFired.set(false);
    }
}

// 布局文件中获取
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="@{(view)->presenter.onClickListenerBinding(employee)}"
    android:text="@{String.valueOf(1+2)}"
    android:visibility="@{employee.isFired?View.VISIBLE:View.GONE}" />

```

* Observable Collections 容器类

```
public class Employee extends BaseObservable implements Serializable {
    public ObservableArrayMap<String, String> user = new ObservableArrayMap<>();
    
    //构造 添加数据
    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        // put 为赋值  get 为取值
        user.put("hello", "world");
        user.put("blog", "blog");
        user.put("Jack", "Jack World");
    }
}

// 布局文件中获取
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text='@{employee.user["hello"]}'
    android:textColor="@color/c_9b6249" />

<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text='@{employee.user["blog"]}'
    android:textColor="@color/c_ff4532" />

<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text='@{employee.user["Jack"]}'
    android:textColor="@color/c_69C614" />

```

### 高级动态绑定

* RecyclerView

* onBindViewHolder


```
final T item = mItems.get(position);
holder.getBinbing().setVariable(BR.item,item);
// 立即绑定，变量和Observable改变后，会在下一个帧进行绑定的改变，如果需要立即执行，可以执行executePendingBindings()
holder.getBinbing().executePendingBindings();
```

## 总结

* data binding 生成规则

```
contact_item.xml -> ContactItemBinding  源码如是
```
* 自定义class
```

<data class="ContactItem">
···
</data>
```

本文是在慕课网上学习的。

1. [Android Data Binding入门](https://www.imooc.com/learn/719) 有兴趣的朋友可以关注学习
2. 推荐大神Blog [Android Data Binding 系列(一) -- 详细介绍与使用](http://connorlin.github.io/2016/07/02/Android-Data-Binding-%E7%B3%BB%E5%88%97-%E4%B8%80-%E8%AF%A6%E7%BB%86%E4%BB%8B%E7%BB%8D%E4%B8%8E%E4%BD%BF%E7%94%A8/)，提供学习。
3. 本文代码在[GitHub项目-LearnDevelopProject](https://github.com/maoai-xianyu/LearnDevelopProject)中，欢迎可以关注。