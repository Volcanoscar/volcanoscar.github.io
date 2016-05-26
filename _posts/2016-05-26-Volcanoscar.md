---
layout: default
title: 你好，世界
---

什么是Intent?
是解决Android应用的各项组件之间的通讯。
它能干些什么?
启动Activity
启动Service
启动BroadcastReceiver
分类
显示Intent通过ComponentName(明确指定类名,直接运行该组件)

构造方式
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivity(intent);
setComponent方式
Intent intent = new Intent();
intent.setComponent(new ComponentName(FirstActivity.this,SecondActivity.class));
setClass方式
Intent intent = new Intent();
intent.setClass(FirstActivity.this,SecondActivity.class);
setClassName方式(一般用于两个app之间跳转)
Intent intent = new Intent();
//第一个参数为需要打开的activity所在app的包名
intent.setClassName("com.demo2","com.demo2.MainActivity");
隐示(并没有明确指定类名,而是通过IntentFilter)

打开网页
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("http://www.baidu.com"));
startActivity(intent);
拨打电话
Intent intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:10086"));
startActivity(intent);
在android5.0以后，启动一个服务必须是显示的Intent
Intent的属性以及IntentFilter
当Intent在组件间传递时，组件如果想告知Android系统自己能够响应和处理哪些Intent，那么就需要用到IntentFilter对象。
除了用于过滤广播的IntentFilter可以在代码中创建外，其他的IntentFilter必须在AndroidManifest.xml文件中进行声明。

ACTION 动作
 Action是指Intent要完成的动作，是一个字符串常量。SDK中定义了一些标准的Action常量如下表所示。
一个Intent只可以设置一个Action，但一个Intentfilter可以持有一个或多个Action用于过滤，到达的Intent只需要匹配其中一个Action即可。 深入思考：如果一个Intentfilter没有设置Action的值，那么，任何一个Intent都不会被通过；反之，如果一个Intent对象没有设置Action值，那么它能通过所有的Intentfilter的Action检查。

ACTION_MAIN
决定应用程序最先启动的Activity
DATA 数据
Intent的Data属性是执行动作的URI和MIME类型，不同的Action有不同的Data数据指定。比如：Action为ACTION_DIAL,那么Data就是Uri.parse("tel:10086") 应该和要拨打的号码 URI Data匹配.
CATEGORY 要执行动作的目标所具有的特质或行为归类
Intent中的Category属性是一个执行动作Action的附加信息。比如：CATEGORY_HOME则表示返回到Home界面.

CATEGORY_BROWSABLE
CATEGORY_BROWSABLE是浏览器在特定条件下可以打开你的activity，比如:我有一个activity，它注册了能显示pdf文档，AndroidManifest.xml内容如下：
<intent-filter>
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="http" android:mimeType="application/pdf"/>
</intent-filter>
这样的话在浏览器中输入或者点击超链接为 http://www.google.com/1.pdf ，那么这个activity自动被浏览器给调用。
CATEGORY_GADGET
...
CATEGORY_HOME
按下home键回到的页面(一般用于launcher的activity)
CATEGORY_LAUNCHER
决定应用程序是否显示在程序列表里
CATEGORY_PREFERENCE
表示该目标Activity是一个首选项界面
CATEGORY_DEFAULT
默认的category
TYPE 要执行动作的目标Activity所能处理的MIME数据类型
Intent的Type属性显式指定Intent的数据类型（MIME）。一般Intent的数据类型能够根据数据本身进行判定，但是通过设置这个属性，可以强制采用显式指定的类型而不再进行推导
例如：一个可以处理图片的目标Activity在其声明中包含这样的mimeType：

<data android:mimeType="image/*">
在使用Intent进行匹配时，我们可以使用setType(String type)或者setDataAndType(Uri data, String type)来设置mimeType。

setType(String type)
setDataAndType(Uri data, String type)
COMPONENT 目标组件的包或类名称
Intent的Compent属性指定Intent的的目标组件的类名称。通常 Android会根据Intent 中包含的其它属性的信息，比如action、data/type、category进行查找，最终找到一个与之匹配的目标组件。但是，如果 component这个属性有指定的话，将直接使用它指定的组件，而不再执行上述查找过程。指定了这个属性以后，Intent的其它所有属性都是可选的。
在分类的显式中已经有过举例
EXTRA 扩展
传递给Intent的额外数据，以Bundle的形式定义，就是一些键值对。
Intent的FLAG
很多是用来指定Android系统如何启动activity，还有启动了activity后如何对待它。
与activity的启动模式相辅相成
FLAG_ACTIVITY_FORWARD_RESULT
多个Activity的值传递。ActivityA到达ActivityB再到达ActivityC，但ActivityB为过渡页可以finish了，此时ActivityC将值透传至ActivityA。
Tips
通过包名获取要跳转的app所对应的intent对象
    Intent intent = getPackageManager().getLaunchIntentForPackage("com.tencent.mm");
    // 这里如果intent为空，就说名没有安装要跳转的应用嘛
    if (intent != null) {
        // 这里跟Activity传递参数一样的嘛，不要担心怎么传递参数，还有接收参数也是跟Activity和Activity传参数一样
        intent.putExtra("key", "value");
        startActivity(intent);
    } else {
        // 没有安装要跳转的app应用，提醒一下
        Toast.makeText(getApplicationContext(), "no install!", Toast.LENGTH_LONG).show();
    }
引用
吃饱的疯子-->Intent详解
开机务农-->Intent
lingdududu--Intent的简介以及属性的详解
Bluestorm-->Android Intent 总结

文／phoenixsky（简书作者）
原文链接：http://www.jianshu.com/p/c563b1e18735
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
