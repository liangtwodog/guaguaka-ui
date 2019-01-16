# 项目介绍
guaguaka-ui是一个基于vue开发的刮刮卡的通用组件

# 引入方式

```
npm i --save guaguaka-ui
```

```
yarn add guaguaka-ui
```
# 使用方法
引入组件

```
import guaguakaUi from 'guaguaka-ui'

```
在components中配置

```
components: {
    guaguakaUi
  }
```

在template中引入
```
<template>
<guaguaka-ui ref="guaguaka" :guaguakaInfo="guaguakaInfo" id="guaguakaDiv" @success="guaguakaSuccess"></guaguaka-ui>
</template>
```
1. guaguakaInfo为刮刮卡初始化的参数对象
-  id：为刮刮卡div的id(必传)
-  width： 设置刮刮卡的宽度（单位px）
-  height：设置刮刮卡的高度（单位px）
-  visibleArea：设置刮刮卡刮开率（单位%）
-  curtainImg：设置刮刮卡覆盖层的图片（必须为同域名下的图片）
-  prizeImg：设置刮刮卡奖品图片
-  isTouch：设置刮刮卡初始化成功后是否可以刮动（true/false）
-  touchSpotShow：设置刮刮卡是否显示刮片（true/false）
-  touchSpotImg：设置刮刮卡刮片的图片
-  touchSpotSize：设置刮刮卡刮动轨迹大小（px），当touchSpotShow为true时为刮片的大小

```
事例
guaguakaInfo: {
        id: 'guaguakaDiv',
        width: 615,
        height: 330,
        visibleArea: 30,
        prizeImg: 'http://t2.hddhhn.com/uploads/tu/201610/198/scx30045vxd.jpg',
        isTouch: true,
        touchSpotSize: 20,
        touchSpotShow: true
      }
```

2. @success()为刮刮卡刮卡成功的回调方法
- 当用户刮开的比率>=初始化所设置的刮开率时，会返回该刮刮卡id（所以刮卡成功的操作可以在这里执行）
3. setInfo()为调用设置刮刮卡信息的方法（需初始化后才可以调用）
-  isTouch：设置刮刮卡是否可以刮动（true/false）
-  touchSpotImg：设置刮刮卡刮片的图片
-  prizeImg：设置刮刮卡奖品图片

```
事例
this.$refs.guaguaka.setInfo({
        prizeImg: 'http://img.zcool.cn/community/01e3eb59a45598a801211d25be7207.jpg@2o.jpg',
        touchSpotImg: 'https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1547195400&di=05806093ea518381728dd6506565ce8b&src=http://img4q.duitang.com/uploads/item/201109/02/20110902193607_CPrxv.thumb.700_0.jpg',
        isTouch: false
      })

设置一张新的刮刮卡可以这样（当isTouch为true时，即刮刮卡生成后可以马上刮动,不传或者为false时则不可刮动）
this.$refs.guaguaka.setInfo({
        prizeImg: 'http://img.zcool.cn/community/01e3eb59a45598a801211d25be7207.jpg@2o.jpg'，
        isTouch: false
      })

```

# 版本
- 1.0.8 增加了使用说明
- 1.0.7 为初始版本

# demo
链接：https://dev.rainchapter.com/seo/

# 问题反馈
这是本人第一个npm包,不足肯定好多！但愿各位大哥哥小姐姐指教，让我有更多的进步。
如有问题联系425874199@qq.com,我们一起解决


