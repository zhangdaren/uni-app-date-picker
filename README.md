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
## 效果图：
![效果图](https://zhangdaren.github.io/uni-app-date-picker/static/date-picker/gif.gif)
   

---
## 更新历史说明：   
v1.0.12(20190523)   
* 修改完成按钮显示逻辑，根据是否选择日期来决定完成按钮显示状态，避免选择单个日期产生的bug(之前参考某网友建议，设置按钮为常开，现修改按钮为按需显示，与美图保持一致，特此说明。)   
* 修改显示逻辑，支持onBackPress()事件
   
v1.0.11(20190523)   
* 修复星期显示问题   
   
v1.0.10(20190523)   
* 隐藏日期上面的星期显示（忙乱中忘记改回去了）   
   
v1.0.9(20190522)   
* 回调里，新增日期的星期参数   
   
v1.0.8(20190522)   
* 新增支持无界面、纯弹窗模式，代码如``` html<calendar :modal="true" :show="showCaledar"></calendar>```，modal设为true，通过属性show来控制显示/隐藏   
* 修复最新小程序报错的问题(Some selectors are not allowed in component wxss, including tag name selectors, ID selectors, and attribute selectors)   
* 修复多次弹出弹窗后，重置天数为1的问题   
* 修复样式问题   
   
v1.0.7(20190521)   
* 解决月星期显示不对的问题，感谢网友指正   
* 适配最新自定义组件编译模式，如遇小程序/app不能正常使用时，请下载最新版   
   
v1.0.6(20190403)   
* 解决日期显示禁用的重大bug（感谢976996406@qq.com反馈）   
* 解决时间显示错误的问题。   
* 近段时间有点忙，时间回显的功能看了看还需要点时间。   
   
v1.0.5(20190322)   
* 小众问题修改   
   
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
* ps：因近段时间较忙，节后将尽快更新。