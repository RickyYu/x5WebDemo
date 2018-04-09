# x5WebDemo
   当前移动端Android主流应用开发都会涉及到H5页面的加载，通常使用Android WebKit下的控件WebView。用起来很方便，但往往会碰到加载缓慢导致用户体验差的问题。我们尝试集成x5内核，验证一下是否真的对加载速度有所优化。

### 一、腾讯x5的优势
1) 速度快：相比系统webview的网页打开速度有**30+%**的提升；
2) 省流量：使用云端优化技术使流量节省**20+%**；
3) 更安全：安全问题可以在24小时内修复；
4) 更稳定：经过亿级用户的使用考验，CRASH率低于**0.15%**；
5) 兼容好：无系统内核的碎片化问题，更少的兼容性问题；
6) 体验优：支持夜间模式、适屏排版、字体设置等浏览增强功能；
7) 功能全：在Html5、ES6上有更完整支持；
8) 更强大：集成强大的视频播放器，支持视频格式远多于系统webview；
9) 视频和文件格式的支持x5内核多于系统内核
10) 防劫持
（以上是腾讯自己所提的一系列优势，看起来还不错）

### 二、项目中集成
1) 第一步：SDK下载，[点击下载](http://x5.tencent.com/tbs/sdk.html) 
![当前提供两种版本选择，根据自己实际情况下载使用](https://upload-images.jianshu.io/upload_images/5443336-35f9dfceb83ed765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 
![下载解压后文件夹内包含以下文件](https://upload-images.jianshu.io/upload_images/5443336-263163bf26cd72df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  以前使用需要申请appkey集成，现在已不需要。直接将上述红框内的jar包放到自己的工程中，可以改下名字，默认的名字太长。
  
2) 第二步：控件使用

* 如果是在老项目中切换使用WebView，在切换到x5可以直接全局更改包名即可，注意不止要在类里面改，xml中的文件也一定要改。
* 如果是集成只需注意导入的包名为com.tencent.**打头即可。

![可以看到，类名基本与原生一致](https://upload-images.jianshu.io/upload_images/5443336-8a0f0ce08813c4bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![xml中使用x5 WebView](https://upload-images.jianshu.io/upload_images/5443336-0bf833670f120ba9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


为了防止遗漏，官方也提供脚本checkqbsdk.sh[点击下载](http://res.imtt.qq.com/TES/checkqbsdk.zip)用于扫描确保替换的完整性，windows 上使用TBSSdk接入扫描工具.exe [点击下载](http://res.imtt.qq.com/TES/TBSSdk_windows.zip) 进行扫描。脚本放在所有源码顶级目录即可。

3) 第三步：配置相关
x5当前不提供64位so文件，但我们可以用下述方式解决该问题
* 打开对应module的build.gradle文件，添加defaultConfig()配置，该操作配置后编译如果报错，则在gradle.properties文件中加上Android.useDeprecatedNdk=true
* 找出build.gradle中配置的so加载目录:jniLibs.srcDir:customerDir,如果没有该项配置则so加载目录默认为：src/main/jniLibs，需要将.so文件都放置在so加载目录的armeabi文件夹下,.so文件可到Demo项目中该目录下去下载,[x5WebDemo](https://github.com/RickyYu/x5WebDemo)地址在文章最底部。
![项目中的配置](https://upload-images.jianshu.io/upload_images/5443336-5122e6a7aca5d25a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![AndroidManifest.xml里加入权限声明](https://upload-images.jianshu.io/upload_images/5443336-1d781f48bf0cf042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4) 第四步 预加载x5内核
建议App在 Application 的 onCreate 中立刻调用 QbSdk 的预加载接口。会提高首次加载网页速度，不会导致白屏现象产生。
![预加载接口 initX5Environment ](https://upload-images.jianshu.io/upload_images/5443336-f6c32c0cb9f0032e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![运行x5WebDemo项目后主界面详情](https://upload-images.jianshu.io/upload_images/5443336-ab834437deaea517.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上配置完后即可在项目中正常使用x5的WebView,有视频、文件相关配置可以查看本项目[x5WebDemo](https://github.com/RickyYu/x5WebDemo)或官方配置文档。


### 三、效果比对
本次测试加载两个web页面，一个京东内购，一个网易严选。为了扩大验证结果，选了一个网速相对较慢的wifi网络。
两个web页面样式如下：
 >![京东内购web](https://upload-images.jianshu.io/upload_images/5443336-9f2c119742ec4d97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![网易严选web](https://upload-images.jianshu.io/upload_images/5443336-bdc485e8b9ab10e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



比对耗时:
>![原生内核-京东加载耗费时长](https://upload-images.jianshu.io/upload_images/5443336-161709c510731104.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![原生内核-网易严选加载消耗时长](https://upload-images.jianshu.io/upload_images/5443336-a78d21590af042ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![x5内核-京东加载消耗时长](https://upload-images.jianshu.io/upload_images/5443336-8d2741ae2f79c274.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![x5内核-网易严选加载消耗时长](https://upload-images.jianshu.io/upload_images/5443336-f43068ae816b5111.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######     可以看到，在切换到x5后，最大的改变就是首次加载页面的速度有大幅度提升，加载时长相比原生快很多。其它时候的加载速度则是有一定的提升，但还无法满足我们的优化需求。

###4、其它
* 去除QQ浏览器推广
 ```
getWindow().getDecorView().addOnLayoutChangeListener(new View.OnLayoutChangeListener() {
       @Override 
         public void onLayoutChange(View v, int left, int top, int right, int bottom, int oldLeft, int oldTop, int oldRight, int oldBottom) { 
                   ArrayList<View> outView = new ArrayList<View>();  
                   getWindow().getDecorView().findViewsWithText(outView,"QQ浏览器",View.FIND_VIEWS_WITH_TEXT); 
                  int size = outView.size(); 
                  if(outView!=null&&outView.size()>0){ 
                        outView.get(0).setVisibility(View.GONE); 
                  } 
             }
          });
```



###5、结语
  实际测试感受，移动端优化加载有限, 若想完美优化web加载速度，提升交互体验。更多还需和前端同学一起配合测试共同优化。
