# OpenCV
## 搭建OpenCV环境
### 下载/编译OpenCV程序源码
* OpenCV官方网站]下载最新版的OpenCV for Linux/Mac源码[http://opencv.org/]
* Ubuntu 14.04 安装软件支持

```
$ sudo apt-get install build-essential cmake libgtk2.0-dev pkg-config python-dev python-numpy libavcodec-dev libavformat-dev libswscale-dev
$ cd opencv-3.1.0
$ mkdir release
$ cd release
//cmake编译OpenCV源码，安装所有的lib文件都会被安装到/usr/local目录下
$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

### 安装
```
$ sudo make install
```
### 下载/编译OpenCV for Java支持库
* OpenCV Java文档]按照说明使用git clone源码 http://docs.opencv.org/2.4/doc/tutorials/introduction/desktop_java/java_dev_intro.html

```
$ git clone git://github.com/Itseez/opencv.git
$ cd opencv
$ git checkout 2.4 //Branch视版本而定
$ mkdir build
$ cd build
 
$ cmake -DBUILD_SHARED_LIBS=OFF ..
//如果报找不到JAVA_HOME环境变量请确认系统是否配置Jdk环境
$ make -j8
//目前编译有误，无法得到jar文件和SO库文件
```
### 下载/编译OpenCV for Android支持库
* OpenCV官方网站]下载最新版的OpenCV for Android源码 http://opencv.org/
* Java JDK
* Android SDK
* Android NDK 
* 安装软件支持

```
$ sudo apt-get install libgtk2.0-dev pkg-config
$ sudo apt-get install build-essential
$ sudo apt-get install cmake
```
### OpenCV for Android 初体验
#### 引入OpenCV Library
注:可能会报错，添加Android API后恢复正常
#### GrayProcess

资源文件：strings.xml
```
<resources>
    <string name="app_name">GrayProcess</string>
    <string name="hello_world">Hello world!</string>
    <string name="menu_settings">Settings</string>
    <string name="title_activity_main">MainActivity</string>
    <string name="str_proc">gray process</string>
    <string name="str_desc">image description</string>
</resources>
```
布局文件：main.xml
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
    
    <Button 
        android:id="@+id/btn_gray_process"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/str_proc"/>
    
    <ImageView
        android:id="@+id/image_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:contentDescription="@string/str_proc"/>

</LinearLayout>
```
Class文件:MainActivity.java
```
package com.iron.grayprocess;

import org.opencv.android.BaseLoaderCallback;
import org.opencv.android.LoaderCallbackInterface;
import org.opencv.android.OpenCVLoader;
import org.opencv.android.Utils;
import org.opencv.core.Mat;
import org.opencv.imgproc.Imgproc;

import android.os.Bundle;
import android.app.Activity;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Bitmap.Config;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.ImageView;

public class MainActivity extends Activity implements OnClickListener{

    private Button btnProc;
	private ImageView imageView;
	private Bitmap bmp;
	
	//OpenCV类库加载并初始化成功后的回调函数，在此我们不进行任何操作
	private BaseLoaderCallback  mLoaderCallback = new BaseLoaderCallback(this) {
        @Override
        public void onManagerConnected(int status) {
            switch (status) {
                case LoaderCallbackInterface.SUCCESS:{
                } break;
                default:{
                    super.onManagerConnected(status);
                } break;
            }
        }
    };
	
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        btnProc = (Button) findViewById(R.id.btn_gray_process);
        imageView = (ImageView) findViewById(R.id.image_view);
        //将lena图像加载程序中并进行显示
        bmp = BitmapFactory.decodeResource(getResources(), R.drawable.lena);
        imageView.setImageBitmap(bmp);
        btnProc.setOnClickListener(this);
    }

	@Override
	public void onClick(View v) {
		Mat rgbMat = new Mat();
		Mat grayMat = new Mat();
		//获取lena彩色图像所对应的像素数据
		Utils.bitmapToMat(bmp, rgbMat);
		//将彩色图像数据转换为灰度图像数据并存储到grayMat中
		Imgproc.cvtColor(rgbMat, grayMat, Imgproc.COLOR_RGB2GRAY);
		//创建一个灰度图像
		Bitmap grayBmp = Bitmap.createBitmap(bmp.getWidth(), bmp.getHeight(), Config.RGB_565);
		//将矩阵grayMat转换为灰度图像
		Utils.matToBitmap(grayMat, grayBmp);
		imageView.setImageBitmap(grayBmp);
	}
	
	@Override
    public void onResume(){
        super.onResume();
        //通过OpenCV引擎服务加载并初始化OpenCV类库，所谓OpenCV引擎服务即是
        //OpenCV_2.4.3.2_Manager_2.4_*.apk程序包，存在于OpenCV安装包的apk目录中
        //这里的OPENCV_VERSION_2_4_3根据库版本来填写
        OpenCVLoader.initAsync(OpenCVLoader.OPENCV_VERSION_2_4_3, this, mLoaderCallback);
    }
}
```

