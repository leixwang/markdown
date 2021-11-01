#APK反编译
##一、下载Apktool+signAPK
链接: <http://pan.baidu.com/s/1boAh9Pt> 密码: k2w6

##二、反编译
```
$ java -jar apktool.jar d 123.apk
```
**d 表示反编译**

**123.apk 表示APK路径**

**反编译结束后会以文件名生成一个文件夹，所有源码文件都在文件夹中**

**反编译完成之后建议先试一试回编防止反编译过程中出错**

##三、修改LOGO和公司签名
1、替换res/drawable文件夹内需要改动的图片文件

2、修改res/values文件夹内的string.xml文件中需要改动的文本。

3、修改AndroidManifest.xml文件，如果是手机版可以不动，如果是平板版，需要将

```
android:screenOrientation="portrait"
改为：
android:screenOrientation="landscape"
```
**注意：尽量不要改动目录结构，不要删除文件和目录，可以替换和修改，除非知道关联关系**

##四、回编
```
$ java -jar apktool.jar b 123
```
**b 表示回编译**

**123 表示反编译后的文件夹路径**

**回编译结束后会在源码文件夹下产生一个dist文件夹，里面就是新生成的APK**

##五、签名
```
$ java -jar signapk.jar platform.x509.pem platform.pk8 broadlink.apk broadlink_signed.apk
```
**刚生成的APK是不能使用的，需要重新签名，签名后才可以安装使用**

