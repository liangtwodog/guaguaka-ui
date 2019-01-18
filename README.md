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
<guaguaka-ui ref="guaguaka" :guaguakaConfig="guaguakaConfig" id="guaguakaDiv" @success="guaguakaSuccess" @hideLoading="hideLoading"></guaguaka-ui>
</template>
```
1. guaguakaConfig为刮刮卡初始化的参数对象
-  id：为刮刮卡div的id(必传)
-  width： 设置刮刮卡的宽度（例如：100px）
-  height：设置刮刮卡的高度（例如：100px）
-  visibleArea：设置刮刮卡刮开率（单位%）
-  curtainImg：设置刮刮卡覆盖层的图片（必须为同域名下的图片）
-  prizeImg：设置刮刮卡奖品图片
-  isTouch：设置刮刮卡初始化成功后是否可以刮动（true/false）
-  touchSpotShow：设置刮刮卡是否显示刮片（true/false）
-  touchSpotImg：设置刮刮卡刮片的图片
-  touchSpotSize：设置刮刮卡刮动轨迹大小（例如：10px）
-  touchSpotImgWidth：设置刮片的宽度（例如：100px）
-  touchSpotImgHeight：设置刮片的高度（例如：100px）
-  touchSpotPosition：设置涂刮的点位于刮片的位置的100%比（例如：0.1 0.1 则 为涂刮的点位于刮片宽度为10%高度为10%的位置）
```
事例
guaguakaConfig: {
        id: 'guaguakaDiv',
        width: 615px,
        height: 330px,
        visibleArea: 30,
        prizeImg: 'http://t2.hddhhn.com/uploads/tu/201610/198/scx30045vxd.jpg',
        isTouch: true,
        touchSpotImg: 'https://i.loli.net/2019/01/17/5c3fef16bbd5a.png',
        touchSpotSize: '20px',
        touchSpotImgWidth: '20px',
        touchSpotImgHeight: '20px',
        touchSpotPosition: '0.5 0.5',
        touchSpotShow: true
      }
```

2. @success()为刮刮卡刮卡成功的回调方法
- 当用户刮开的比率>=初始化所设置的刮开率时，会返回该刮刮卡id（所以刮卡成功的操作可以在这里执行）
3. @hideLoading()为刮刮卡加载完成的回调方法
- 当刮刮卡覆盖层和奖品层设置好时返回该刮刮卡id(所有隐藏loading可以在这里执行)
4. setInfo()为调用设置刮刮卡信息的方法（需初始化后才可以调用）
-  isTouch：设置刮刮卡是否可以刮动（true/false）
-  touchSpotImg：设置刮刮卡刮片的图片
-  touchSpotSize：设置刮刮卡刮动轨迹大小（例如：10px）
-  touchSpotImgWidth：设置刮片的宽度（例如：100px）
-  touchSpotImgHeight：设置刮片的高度（例如：100px）
-  touchSpotPosition：设置涂刮的点位于刮片的位置的100%比（例如：0.1 0.1 则 为涂刮的点位于刮片宽度为10%高度为10%的位置）
-  prizeImg：设置刮刮卡奖品图片

```
事例
this.$refs.guaguaka.setInfo({
        prizeImg: 'http://img.zcool.cn/community/01e3eb59a45598a801211d25be7207.jpg@2o.jpg',
        touchSpotImg: 'https://i.loli.net/2019/01/17/5c3ffe3dbafa3.png',
        touchSpotImgWidth: '30px',
        touchSpotImgHeight: '30px',
        touchSpotPosition: '0.5 0.5',
        touchSpotSize: '20px',
        isTouch: false
      })

设置一张新的刮刮卡可以这样（当isTouch为true时，即刮刮卡生成后可以马上刮动,不传或者为false时则不可刮动）
this.$refs.guaguaka.setInfo({
        prizeImg: 'http://img.zcool.cn/community/01e3eb59a45598a801211d25be7207.jpg@2o.jpg'，
        isTouch: false
      })

```

# 版本
- 1.1.1
1. 修改了在PC端时当鼠标移出刮刮卡时，刮片不消失的问题
- 1.0.9
1. 新增了刮刮卡加载完成的回调方法hideLoading()，方便隐藏loading
2. 刮刮卡配置信息的width和height修改可以设置为100px,10rem,100%
3. 默认刮片图片和默认奖品层图片修改为链接形式引入
4. 配置刮刮卡时新增了可以设置刮片的大少和涂刮点位于刮片的位置
5. setInfo()设置刮刮卡信息的方法，新增了可以设置刮片的大少，设置刮片刮动轨迹的大少，设置涂刮点位于刮片的位置
- 1.0.8 增加了使用说明
- 1.0.7 为初始版本

# demo
链接：https://liangtwodog.github.io/guaguakaDemo/#/

# 问题反馈
这是本人第一个npm包,不足肯定好多！但愿各位大哥哥小姐姐指教，让我有更多的进步。
如有问题联系425874199@qq.com,我们一起解决


