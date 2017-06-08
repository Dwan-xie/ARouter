```
    Android平台中对页面、服务提供路由功能的中间件，我的目标是 —— 简单且够用。
```

本项目完全基于ARouter，因项目中引入React-native导致support-v4包无法升级到25.2.0（理论上24.+即可），顾用最小的修改量兼容了23+的情况。

**1.修改Activity启动方式**
```
// Navigation in main looper.
new Handler(Looper.getMainLooper()).post(new Runnable() {
    @Override
    public void run() {
        if (requestCode > 0) {  // Need start for result
            ActivityCompat.startActivityForResult((Activity) currentContext, intent, requestCode, postcard.getOptionsBundle());
        } else {
            ActivityCompat.startActivities(currentContext, new Intent[]{intent}, postcard.getOptionsBundle());
        }

        if ((0 != postcard.getEnterAnim() || 0 != postcard.getExitAnim()) && currentContext instanceof Activity) {    // Old version.
            ((Activity) currentContext).overridePendingTransition(postcard.getEnterAnim(), postcard.getExitAnim());
        }

        if (null != callback) { // Navigation over.
            callback.onArrival(postcard);
        }
    }
});
```
**将**
```
ActivityCompat.startActivity(currentContext, intent, postcard.getOptionsBundle());}
```
**替换为**
```
ActivityCompat.startActivities(currentContext, new Intent[]{intent}, postcard.getOptionsBundle());}
```

2.替换Postcard中RequiresApi注解
**将**
```
@RequiresApi(16)
```
**替换为**
```
@TargetApi(16)
```

具体的使用方法和问题，还请参考原库(https://github.com/alibaba/ARouter)

#### 最新版本

模块|arouter-api|arouter-compiler|arouter-annotation
---|---|---|---
当前版本|1.2.1.1|1.1.2.1|1.0.3





