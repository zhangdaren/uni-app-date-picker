# uni-app-date-picker
uni-app 酒店日期选择插件
   
功能基本参照美团APP来实现的，期间因uniapp框架限制，判断代码只能长串写在标签上，导致元素也增了许多，性能和操控受影响，因而将滑动固定月份标题功能砍掉了，后续再考虑加入。
   
说明：目前支持APP、微信小程序和H5平台。其他平台未测试，理论支持   
   
部分代码来源于：[https://github.com/zrxisme/date-for-wx](https://github.com/zrxisme/date-for-wx)。   
   
反馈问题请描述所使用的平台和重现步骤，谢谢~~   
   
   
---
## 回调函数示例：

``` html
<calendar @change="change"></calendar>
   
   
   
   
methods: {
	change({choiceDate, dayCount})
	{
		//1.choiceDate 时间区间（开始时间和结束时间）
		//2.dayCount 共多少晚
		
		console.dir(choiceDate)
		console.log("入住从 "+ choiceDate[0].re + "  到 " + choiceDate[1].re + "  共 " + dayCount +" 晚");
	}
}
```
   
   
---
## 效果图：
![效果图](https://zhangdaren.github.io/uni-app-date-picker/static/date-picker/gif.gif)
   

---
## 更新历史说明：   
v1.0.4   
* 组件内view设置为flex布局，防止样式污染   
   
v1.0.3   
* 回调参数修改   
   
v1.0.2   
* 支持H5平台   
   
v1.0.1   
* 增加change事件，回调函数里返回所选区间和入住的时间（即几晚）   
* 限制只能点击确认按钮选择日期，点击遮罩或点击x都会重置时间到选择之前的数据   
   
v1.0.0   
* 初次提交   
   
   
## Future   
* 无标签式启动   
* 配置参数：颜色