# -
#1、使用AIDL调用其他应用的服务。
如客户端调用服务器端的服务
首先在服务器端定义aidl的接口，后缀为aidl,然后在service中的onbind()函数中返回接口的实现类对象。
客户端在相同包下建立相同的aidl接口，然后bindservice，在serviceconnection 类中的onserviceconnected(),中使用service=LocalService.Stub.asInterface(binder); 获取接口实现对象
#2、引用系统为公开资源
Android中没有公开的资源，在xml中直接引用会报错。除了去找到对应资源并拷贝到我们自己的应用目录下使用以外，我们还可以将引用“@android”改成“@*android”解决。比如上面引用的附件图标，可以修改成下面的代码。

android:icon="@*android:drawable/ic_menu_attachment"

修改后，再次Build工程，就不会报错了。
#3、javadoc生成乱码
###Android studio生成javadoc文件乱码怎么办?　　  
一般使用Android Studio生成javadoc会有两个问题：　  
　空指针异常　  
　文档乱码　  
　解决办法如下：　  
　第1个问题：Tools --> Generate JavaDoc -->打开对话框活，在"Other command line arguments"输入 “-bootclasspath /Users/用户名/sdk/platforms/android-14/android.jar”，红色部分每个人电脑不尽相同，指定android.jar的位置就行。　  
　第2个问题：同上打开对话框后，在"Other command line arguments"追加输入(参数之间勿忘空格)“-encoding utf-8 -charset utf-8”　　经过这样设置后，已经能够成功生成文档。期间有一些报错和警告，根据提示修改即可。　  
　当然，使用jdk1.6的时候问题较多，升级到 jdk1.7+后，会顺利很多，警告和错误要少很多
#4、下拉刷新框架android-Ultra-Pull-To-Refresh导入到Android Studio
##
参考网址http://www.ithao123.cn/content-10797769.html

pom.xml

<dependency>
  <groupId>in.srain.cube</groupId>
            <artifactId>cube-sdk</artifactId>
            <type>aar</type>
            <version>1.0.44.38</version>
  </dependency>

 build.gradle

dependencies {
    compile(project(':ptr-lib')) {
    }
    compile 'in.srain.cube:clog:1.0.2'
    //compile 'in.srain.cube:cube-sdk:1.0.44.39-SNAPSHOT@aar'
    compile 'in.srain.cube:cube-sdk:1.0.44.38'
    compile 'com.google.android:support-v4:r7'
}
#5、Error:Execution failed for task ':app:transformResourcesWithMergeJavaResForDebug'.
> com.android.build.api.transform.TransformException: com.android.builder.packaging.DuplicateFileException: Duplicate files copied in APK META-INF/NOTICE
	File1: H:\Users\vzhihao\AndroidStudioProjects\VideoPlayer\app\libs\httpclient-4.3.5.jar
	File2: H:\Users\vzhihao\AndroidStudioProjects\VideoPlayer\app\libs\httpmime-4.3.5.jar
	File3: H:\Users\vzhihao\AndroidStudioProjects\VideoPlayer\app\libs\httpcore-4.3.2.jar
在build.gradle中添加
	compileSdkVersion 23
    buildToolsVersion "23.0.2"
    packagingOptions {
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/NOTICE'
        exclude 'META-INF/DEPENDENCIES'
    }
#6、actionBar+PagerView
actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);空指针异常
更改主题为Android:theme.light
#查找 [代码网站][1]

[1]: http://grepcode.com/

#tab的几种方式[网址][2],demo[下载地址][3]
[2]:http://blog.csdn.net/crazy1235/article/details/42678877
[3]:http://download.csdn.net/detail/crazy1235/8358671
#遇错 [解决网址][4]
java.lang.IllegalArgumentException: Service Intent must be explicit: Intent { act=com.example.raid.service.FIRST_SERVICE }
##
  因为版本的原因，有些时候我们使用Service的时需要采用隐私启动的方式，但是Android 5.0一出来后，其中有个特性就是Service Intent  must be explitict，也就是说从Lollipop开始，service服务必须采用显示方式启动。
##

5.0前的版本只需要如下代码即可开启服务： 

final Intent intent = new Intent();
intent.setAction("com.example.raid.service.FIRST_SERVICE"); 

但是现在需要加上一个setPackage方法：

final Intent intent = new Intent();
        intent.setAction("com.example.raid.service.FIRST_SERVICE");
        intent.setPackage(getPackageName());
##
[4]:http://www.ithao123.cn/content-10219582.html
##7、android studio更新导致一直refreshing
###在下载相应的gradle,路径（.gradle/wrapper/）
gradle下载网址：http://services.gradle.org/distributions

##8、使用webview注意问题
###
1. 添加权限<uses-permission android:name="android.permission.INTERNET" />
2. 出现:net::err_cache错误 
#
	if (Build.VERSION.SDK_INT >= 19) {
            mWebView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
        }
3. 加载js 
#
	WebSettings webSettings = myWebView.getSettings();
	webSettings.setJavaScriptEnabled(true);
#9、[Android Studio中获取SHA1或MD5的方法 （百度地图）](http://www.imooc.com/article/6899 "Android Studio中获取SHA1或MD5的方法 （百度地图）")
#
	keytool -v -list -keystore H:\Users\vzhihao\.android\debug.keystore
#10、使用TabActivity注意事项
1. TabHost必须包含一个 TabWidget和一个FrameLayout
2. FrameLayout的id属性必须为 @android:id/tabcontent
3. TabWidget的id属性必须为 @android:id/tabs
#11、Fragment Touch事件泄露
##
当Fragment的栈里面有几个fragment的时候，这个时候如果是几个fragment状态是hide，当你触摸当前fragment的时候，下层的fragment的事件被触发，这是由于Touch事件泄露传到了下层中。解决方法就是拦截fragment的 

解决办法：

// onTouch事件 将上层的触摸事件拦截 

	public class GroupApplyFragment extends Fragment 
	 implements OnTouchListener { 

    @Override
    public boolean onTouch(View v, MotionEvent event) {
        return true;
    }
 
    @Override
    public void onViewCreated(View view, Bundle savedInstanceState) {
        // 拦截触摸事件，防止泄露下去
        view.setOnTouchListener(this);
    }
#12、[fragment嵌套fragment问题](http://bbs.9ria.com/thread-259216-1-1.html )
当遇到这种嵌套多层的Fragment的时候第一个主要点就是第一层的FragmentManager，可以通过getSupportFragmentManager()或者getFragmentManager()获得，但是在第二层的fragment中如果想获得FragmentManager就不能这样了，必须用getChildFragmentManager()。


	