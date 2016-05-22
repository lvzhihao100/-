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
　当然，使用jdk1.6的时候问题较多，升级到jdk1.7+后，会顺利很多，警告和错误要少很多
