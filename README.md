# uni-app-date-picker
uni-app 酒店日期选择插件
   
功能基本参照美团APP来实现的，期间因uniapp框架限制，判断代码只能长串写在标签上，导致元素也增了许多，性能和操控受影响，因而将滑动固定月份标题功能砍掉了，后续再考虑加入。
   
说明：目前支持APP、微信小程序和H5平台。其他平台未测试，理论支持   

   
##### 大更新：新增支持无界面、纯弹窗模式。
##### 代码如：
``` html<calendar :modal="true" :show="showCaledar"></calendar>```
##### 使用方式：modal设为true，通过属性show来控制显示/隐藏   
   
   
部分代码来源于：[https://github.com/zrxisme/date-for-wx](https://github.com/zrxisme/date-for-wx)。   
   
反馈问题请描述所使用的平台和重现步骤，谢谢~~   
   
   
---
## 回调函数示例：

``` html
<!-- 组件模式(有界面)  -->
<calendar @change="change"></calendar>
   
   
<!-- 弹窗模式(无界面) -->
<calendar @change="change2" :modal="true" :show="showCaledar"></calendar>

<text class="text-center dateStr">{{ dateStr }}</text>
<button class="btn" @tap="showCaledar = !showCaledar">显示/隐藏</button>
``` 
   
   
   
``` javascript
export default {
	data() {
		return {
			showCaledar: false,
			dateStr: ''
		};
	},
	components: {
		calendar
	},
	methods: {
		change({ choiceDate, dayCount }) {
			//参数解释
			//1.choiceDate 时间区间（开始时间和结束时间）
			//2.dayCount 共多少晚

			console.log(choiceDate, dayCount);
			console.log('入住从 ' + choiceDate[0].re + '  到 ' + choiceDate[1].re + '  共 ' + dayCount + ' 晚');
		},
		change2({ choiceDate, dayCount }) {
			this.dateStr = '入住从 ' + choiceDate[0].re + ' (星期' + choiceDate[0].week + ')  到 ' + choiceDate[1].re + '(星期' + choiceDate[1].week + ')' + '  共 ' + dayCount + ' 晚 ';
		}
	}
}
```
   
    
---
## 参数示例：
| 属性名 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| modal | Boolean | false | 是否为弹窗模式，默认为false|
| show | Boolean | false | 是否显示弹窗(与modal一起使用)，默认为false|
| singleDate | Boolean | false | 表示是否只选择一个日期，默认为false |
| startDate | String | 今天 | 开始时间，默认为今天|
| endDate |String |明天 | 开始时间，默认为明天 |
| daysCount | String | 150天 | 显示的天数范围: 60 < x < 360(从今天算起) |
| change | EventHandle | | 选择日期后的回调事件，event.detail = {choiceDate: value1,dayCount:value2} |

---
## 效果图：
![效果图](https://zhangdaren.github.io/uni-app-date-picker/static/date-picker/gif.gif)
   

## TODO
* 选中的日期可能会被挡掉
* 语种
* nvue版本
* 历史时间选择(优先级低)
-------------------------
弹窗模式上，不能根据show变量来显示/隐藏
------------------------

---
## 更新历史说明：   
## v1.0.19(20200110)   
* 增加法定节假日和补班的显示，替换之前的假日显示，更符合国情
* 优化dayCount为从今天开始算起 

## v1.0.18(20191107)   
* 修复单个日期显示错误   
* 修复app平台上弹窗模式，没有完成按钮   
