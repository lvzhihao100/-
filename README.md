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
#actionBar+PagerView
actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);空指针异常
更改主题为Android:theme.light
#查找 [代码网站][1]

[1]: http://grepcode.com/

#tab的几种方式[网址][2],demo[下载地址][3]
[2]:http://blog.csdn.net/crazy1235/article/details/42678877
[3]:http://download.csdn.net/detail/crazy1235/8358671
