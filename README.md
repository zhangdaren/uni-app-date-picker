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
| daysCount | String | 150天 | 显示的天数范围: 60 < x < 360 |
| change | EventHandle | | 选择日期后的回调事件，event.detail = {choiceDate: value1,dayCount:value2} |

---
## 效果图：
![效果图](https://zhangdaren.github.io/uni-app-date-picker/static/date-picker/gif.gif)
   

## TODO
* 选中的日期可能会被挡掉
* 语种
* nvue版本
* 历史时间选择(优先级低)

---
## 更新历史说明：   
## v1.0.17(20191020)   
* 新增动态调整初始日期，调整startDate和endDate绑定的值即可，示例有做描述   
   
## v1.0.16(20190928)   
* 修复H5确认按钮被覆盖的bug   
   
## v1.0.15(20190824)   
* 新增singleDate模式，可以选择单个日期，赋值使用startDate，后面会支持选择今天之前的日期(架构需要做大的调整才行)
* 修复非今年的星期有误的bug   

### v1.0.14(20190613)   
* 修复星期不对的问题(┑(￣Д ￣)┍,几个版本终于修复好)   
* 小修小补，支持显示今天、明天、后天。   
   
### v1.0.13(20190604)   
* [新增] 主题颜色支持修改：修改date-picker.vue里$themeColor的值即可，暂未想到更好的方法，欢迎给意见~   
* [新增] 支持回填数据，参数为startDate和endDate，仅支持字符串，如"2019-06-05"，已做好防错处理(如小于今天的情况，开始时间在结束时间之后的情况等)   
* [新增] 支持修改总共显示多少天，参数daysCount，范围为60 < x < 360，考虑到性能，将最大天数限制在360天内   
* 修复首月星期显示不对的问题   
   
### v1.0.12(20190523)   
* 修改完成按钮显示逻辑，根据是否选择日期来决定完成按钮显示状态，避免选择单个日期产生的bug(之前参考某网友建议，设置按钮为常开，现修改按钮为按需显示，与美图保持一致，特此说明。)   
* 修改显示逻辑，支持onBackPress()事件
   
### v1.0.11(20190523)   
* 修复星期显示问题   
   
### v1.0.10(20190523)   
* 隐藏日期上面的星期显示（忙乱中忘记改回去了）   
   
### v1.0.9(20190522)   
* 回调里，新增日期的星期参数   
   
### v1.0.8(20190522)   
* 新增支持无界面、纯弹窗模式，代码如``` html<calendar :modal="true" :show="showCaledar"></calendar>```，modal设为true，通过属性show来控制显示/隐藏   
* 修复最新小程序报错的问题(Some selectors are not allowed in component wxss, including tag name selectors, ID selectors, and attribute selectors)   
* 修复多次弹出弹窗后，重置天数为1的问题   
* 修复样式问题   
   
### v1.0.7(20190521)   
* 解决月星期显示不对的问题，感谢网友指正   
* 适配最新自定义组件编译模式，如遇小程序/app不能正常使用时，请下载最新版   
   
### v1.0.6(20190403)   
* 解决日期显示禁用的重大bug（感谢976996406@qq.com反馈）   
* 解决时间显示错误的问题。   
* 近段时间有点忙，时间回显的功能看了看还需要点时间。   
   
### v1.0.5(20190322)   
* 小众问题修改   
   
### v1.0.4   
* 组件内view设置为flex布局，防止样式污染   
   
### v1.0.3   
* 回调参数修改   
   
### v1.0.2   
* 支持H5平台   
   
### v1.0.1   
* 增加change事件，回调函数里返回所选区间和入住的时间（即几晚）   
* 限制只能点击确认按钮选择日期，点击遮罩或点击x都会重置时间到选择之前的数据   
   
### v1.0.0   
* 初次提交   
   
   
## Future   
* 无标签式启动   
* 配置参数：颜色
* ps：因近段时间较忙，节后将尽快更新。