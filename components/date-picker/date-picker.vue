<template>
	<view>
		<view class='order-time' @tap="showCalendar()">
			<text class='goInHotel'>入住</text>
			<text class='date-wrappper'>{{choiceDate[0].month}}月{{choiceDate[0].day}}日</text>
			<text class='goInHotel2' v-if="choiceDate[0].year+choiceDate[0].month+choiceDate[0].day==today">今天</text>
			<text class='left-hotel'>离店</text>
			<text class='date-wrappper'>{{choiceDate[choiceDate.length-1].month}}月{{choiceDate[choiceDate.length-1].day}}日</text>
			<text class='goInHotel2' v-if="choiceDate[choiceDate.length-1].year+choiceDate[choiceDate.length-1].month+choiceDate[choiceDate.length-1].day==tomorrow">明天</text>
			<text class='sumCount'>{{dayCount2}}</text>
		</view>

		<view class='calendar-layer' :animation='animationData'>
			<!-- 遮罩层 -->
			<view class='layer-white-space' @tap='hideCalendar'></view>

			<view class='layer-content' :class="choice ? 'choiceDate' : ''">
				<view class="layer-header">
					<icon class="layer-close" @tap='hideCalendar'></icon>
					<text class="layer-title">选择日期</text>
				</view>
				<!--  -->
				<view class="layer-body">
					<view class="week-box">
						<text class="week-box-item" v-for="(item,tmpIndex) in weekNameArr" :key='tmpIndex'>{{item}}</text>
					</view>
					<!--  -->
					<scroll-view class='layer-list' scroll-y="true">
						<view v-for="(monthData,index) in date" :key="index" class="month">
							<view class="month-title" :id="'m-' + monthData[0].year + '-' + monthData[0].month" :style="'z-index:' + index">{{monthData[0].year +'年' + monthData[0].month + '月'}}</view>
							<view class="month-content">
								<view v-for="(data,index2) in monthData" :key="index2" class="day" :data-index="index" :data-indexs="index2"
								 :class="data.date < today ? 'disabled' : data.selected ? 'active' + (data.date == choiceDate[0].date ? ' begin' : data.date == choiceDate[1].date ? ' end' : '') : ''"
								 :style="index2 == 0 ? 'margin-left:' +(weeks[index] == 6 ? 0 : weeks[index]) * (100 / 7) + '%':''" @tap="selectday">
									<view class="day-content">
										<text class="day-subject">{{(data.date != today) && (data.date != tomorrow) && (data.date != afterTomorrow) ? data.act.subject :'' }}</text>
										<text class="day-txt">{{data.date == today ? '今天' : (data.date == tomorrow ? '明天' : (data.date == afterTomorrow ? '后天' : data.day))}}</text>
										<text class="day-tip">{{data.act.tip}}</text>
									</view>
									<view class="beginTip" v-if="choice===false">请选择离店日期</view>
									<view class="endTip" v-if="choice">{{dayCount2}}</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
				<!--  -->
				<view class='layer-footer'>
					<view class="submitBtn" @tap="submitbtn">完成</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				date: [],
				weeks: [],
				dayCount: 1,
				dayCount2: "共1晚",
				festival: {
					"101": "元旦",
					"214": "情人节",
					"308": "妇女节",
					"312": "植树节",
					"315": "消费者权益日",
					"401": "愚人节",
					"405": "清明节",
					"501": "劳动节",
					"504": "青年节",
					"512": "护士节",
					"601": "儿童节",
					"701": "建党节",
					"801": "建军节",
					"910": "教师节",
					"928": "孔子诞辰",
					"1001": "国庆节",
					"1006": "老人节",
					"1024": "联合国日",
					"1224": "平安夜",
					"1225": "圣诞节"
				},
				haveOrder: [],
				dateFlag: {},
				choice: "",
				today: 0,
				choiceDate: [],
				choiceDateArr: [],
				tomorrow: 0,
				afterTomorrow: 0,
				weekNameArr: ['日', '一', '二', '三', '四', '五', '六'],
				animationData: null,
				curScrollTop: 0,
				monthTitleRectList: [],
				fixedId: '',
				layerTop: 0,
				isShowDialog: false,
			}
		},
		components: {},
		onLoad() {
			// 弹出层动画创建
			this.animation = uni.createAnimation({
				duration: 400, // 整个动画过程花费的时间，单位为毫秒
				timingFunction: "ease", // 动画的类型
				delay: 0 // 动画延迟参数
			})
			this.dateData();
			// this.showCalendar();
		},
		methods: {
			getLayerTop: function() {
				return new Promise(resolve => {
					//获取layer-list窗器的top
					let view2 = uni.createSelectorQuery().select(".layer-list");
					view2.boundingClientRect(data => {
						resolve(data.top);
					}).exec();
				});
			},
			getMonthTitleRectList: function() {
				return new Promise(resolve => {
					//获取每个月的文字头的top
					let view = uni.createSelectorQuery().selectAll(".month-title");
					view.boundingClientRect(data => {
						resolve(data);
					}).exec();
				});
			},
			getRectList: async function() {
				if (!this.layerTop || this.layerTop < 0) this.layerTop = await this.getLayerTop();
				var isInitRectData = true;
				if (this.monthTitleRectList && this.monthTitleRectList.length > 0) {
					if (this.monthTitleRectList[0].top == 0) {
						this.monthTitleRectList = await this.getMonthTitleRectList();
						isInitRectData = false;
					}
				} else {
					isInitRectData = false;
				}
				if (isInitRectData == false) this.monthTitleRectList = await this.getMonthTitleRectList();
			},
			showCalendar: function() {
				// 设置动画内容为：使用绝对定位显示区域，高度变为100%
				this.animation.bottom("0px").height("100%").step();
				this.animationData = this.animation.export();
				setTimeout(() => {
					this.isShowDialog = true;
				}, 400);
			},
			hideCalendar: function() {
				// 设置动画内容为：使用绝对定位隐藏整个区域，高度变为0
				this.animation.bottom("-100%").height("0upx").step();
				this.animationData = this.animation.export();
				this.isShowDialog = false;
			},
			setData: function(obj) {
				let that = this;
				let keys = [];
				let val, data;
				Object.keys(obj).forEach(function(key) {
					keys = key.split('.');
					val = obj[key];
					data = that.$data;
					keys.forEach(function(key2, index) {
						if (index + 1 == keys.length) {
							that.$set(data, key2, val);
						} else {
							if (!data[key2]) {
								that.$set(data, key2, {});
							}
						}
						data = data[key2];
					})
				});
			},
			onScroll: function(e) {
				//////暂时还有问题，效率也不太好，等后缀再优化
				//<scroll-view class='layer-list' scroll-y="true" @scroll="onScroll">
				//<view v-for="(monthData,index) in date" :key="index" class="month" :class="fixedId == ('m-' + monthData[0].year + '-' + monthData[0].month) ? 'fixed' : ''">
				//
				// this.getRectList();
				// 
				// let scorllTop = e.detail.scrollTop;
				// this.curScrollTop = scorllTop + this.layerTop;
				// 
				// ///////////////////////////////////////////
				// this.monthTitleRectList.forEach((item, i) => {
				// 	if (this.curScrollTop > item.top - 70) {
				// 		this.fixedId = item.id;
				// 		///////////这里理应需要 节流 和 return，后面再处理/////////////
				// 	}
				// });
			},
			dateData: function() {
				let dataAll = []; //总日历数据
				let dataAll2 = []; //总日历数据
				let dataMonth = []; //月日历数据
				let date = new Date(); //当前日期
				let getDateTime = date.getTime();
				let year = date.getFullYear(); //当前年
				let week = date.getDay(); //当天星期几
				let weeks = [];
				let month = date.getMonth() + 1; //当前月份
				let day = date.getDate(); //当天
				let daysCount = 150; //一共显示多少天
				let dayscNow = 0; //计数器
				let monthList = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]; //月份列表
				let nowMonthList = []; //本年剩余年份
				this.today = "" + year + month + day;
				this.tomorrow = "" + year + month + (day + 1);
				this.afterTomorrow = "" + year + month + (day + 2);

				for (let i = month; i < 13; i++) {
					nowMonthList.push(i)
				}
				let yearList = [year] //年份最大可能
				for (let i = 0; i < daysCount / 365 + 2; i++) {
					yearList.push(year + i + 1)
				}
				let leapYear = function(Year) { //判断是否闰年 
					if (((Year % 4) == 0) && ((Year % 100) != 0) || ((Year % 400) == 0)) {
						return (true);
					} else {
						return (false);
					}
				}
				for (let i = 0; i < yearList.length; i++) { //遍历年
					let mList
					if (yearList[i] == year) { //判断当前年份
						mList = nowMonthList
					} else {
						mList = monthList
					}
					for (let j = 0; j < mList.length; j++) { //循环月份
						dataMonth = []
						let t_days = [31, 28 + leapYear(yearList[i]), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
						let t_days_thisYear = []
						if (yearList[i] == year) {
							for (let m = 0; m < nowMonthList.length; m++) {
								t_days_thisYear.push(t_days[mList[m] - 1])
							}
							t_days = t_days_thisYear
						} else {
							t_days = [31, 28 + leapYear(yearList[i]), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
						}

						for (let k = 0; k < t_days[j]; k++) { //循环每天
							dayscNow++;
							let nowData;
							if (dayscNow < daysCount) { //如果计数器没满
								let days = k + 1;
								if (days < 10) {
									days = "0" + days;
								}

								var monthAndDay = mList[j] + "" + days;
								var dateString = yearList[i] + "-" + mList[j] + "-" + days;

								var subject = this.festival[monthAndDay];
								var none = false;
								let newDateFormate = new Date(yearList[i] + "/" + mList[j] + "/" + (k + 1));
								let thisDateTime = newDateFormate.getTime();
								var selected = 0;
								if (yearList[i] == year && mList[j] == month) { //判断当年当月
									if (k + 1 >= day) {
										nowData = {
											year: yearList[i],
											month: mList[j],
											act: {
												subject,
												none,
												tip: '',
												defaultStr: 0
											},
											day: k + 1,
											date: yearList[i] + "" + mList[j] + days,
											selected,
											re: yearList[i] + "-" + mList[j] + "-" + days,
											dateTime: thisDateTime
										}
										dataMonth.push(nowData)
										if (k + 1 == day) {
											let date = new Date(yearList[i] + "/" + mList[j] + "/" + (k + 1));
											let weekss = date.getDay(); //获取每个月第一天是周几
											weeks.push(weekss);
										}
									}
								} else { //其他情况
									nowData = { //组装自己需要的数据
										year: yearList[i],
										month: mList[j],
										act: {
											subject,
											none,
											tip: '',
											defaultStr: 0
										},
										day: k + 1,
										date: yearList[i] + "" + mList[j] + days,
										selected,
										re: yearList[i] + "-" + mList[j] + "-" + days,
										dateTime: thisDateTime
									}
									dataMonth.push(nowData);
									if (k == 0) {
										let date = new Date(yearList[i] + "/" + mList[j] + "/" + (k + 1));
										let weekss = date.getDay(); //获取每个月第一天是周几
										weeks.push(weekss);
									}
								}
							} else {
								break;
							}
						}
						dataAll.push(dataMonth);
					}
				}
				for (let i = 0; i < dataAll.length; i++) {
					if (dataAll[i].length != 0) {
						dataAll2.push(dataAll[i]);
					}
				}
				dataAll2[0][0].selected = 1;
				dataAll2[0][0].act.tip = "入住";
				dataAll2[0][0].act.defaultStr = 1;
				this.choiceDate.push(dataAll2[0][0]);
				if (dataAll2[0][1]) {
					dataAll2[0][1].selected = 1;
					dataAll2[0][1].act.tip = "离店";
					dataAll2[0][1].act.defaultStr = 1;
					this.choiceDate.push(dataAll2[0][1]);
				} else {
					dataAll2[1][0].selected = 1;
					dataAll2[1][0].act.tip = "离店";
					dataAll2[1][0].act.defaultStr = 1;
					this.choiceDate.push(dataAll2[1][0]);
				}
				console.log(dataAll2, weeks, this.today, this.tomorrow, this.afterTomorrow);
				this.date = dataAll2;
				this.weeks = weeks;
				this.choiceDate = this.choiceDate;
				this.choiceDateArr = this.choiceDate;
			},
			selectday: function(e) {
				var indexs = e.currentTarget.dataset.indexs;
				var index = e.currentTarget.dataset.index;

				if (indexs == -1) {
					return
				}
				this.date[index][indexs].selected = 1;
				this.date[index][indexs].act.tip = "入住";
				if (this.dateFlag.date && this.date[index][indexs].dateTime < this.dateFlag.date.dateTime) {
					var flagIndex = this.dateFlag.index;
					var flagIndexs = this.dateFlag.indexs;
					this.date[flagIndex][flagIndexs].selected = 0;
					this.date[flagIndex][flagIndexs].act.tip = "";
					this.dateFlag = {
						date: this.date[index][indexs],
						index: index,
						indexs: indexs
					};
					this.choice = false;
					this.dayCount = 1;
					//
					this.choiceDate[0] = this.date[index][indexs];
				} else if (this.dateFlag && this.dateFlag.date) {
					if (this.dateFlag.index == index && this.dateFlag.indexs == indexs) {
						return;
					}
					this.date[index][indexs].act.tip = "离店";
					//
					var that = this;
					var dateFlagDateTime = that.dateFlag.date.dateTime;
					var choiceDateTime = that.date[index][indexs].dateTime;
					that.choiceDateArr = [];
					that.choiceDateArr.push(that.dateFlag.date);
					var nonFlag = false;
					var nonArr = [];
					var count = 0;
					this.date.forEach(function(dataItems) {
						dataItems.forEach(function(dataItem) {
							if (dataItem.dateTime > dateFlagDateTime && dataItem.dateTime < choiceDateTime) {
								if (dataItem.act.none) {
									nonFlag = true;
									nonArr.push(dataItem.day);
								}
								that.choiceDateArr.push(dataItem);
								dataItem.selected = 1;
								count++;
							}
						})
					})
					that.choiceDateArr.push(that.date[index][indexs]);
					//设置开始和结果两个日期
					this.choiceDate[0] = that.choiceDateArr[0];
					this.choiceDate[1] = that.choiceDateArr[that.choiceDateArr.length - 1];
					//
					if (nonFlag) {
						var that = this;
						this.date.forEach(function(dataItems) {
							dataItems.forEach(function(dataItem) {
								if (dataItem.dateTime != choiceDateTime) {
									dataItem.act.tip = ""
									dataItem.selected = 0
								} else {
									dataItem.act.tip = "入住"
								}
							})
						})
						this.dateFlag = {
							date: that.date[index][indexs],
							index: index,
							indexs: indexs
						};
						this.choiceDate[0] = that.date[index][indexs];
						this.dayCount = 1;
						var nonstr = "";
						nonArr.forEach(function(nonitem, index) {

							if (index != nonArr.length - 1) {
								nonstr = nonstr + nonitem + "号,"
							} else {
								nonstr = nonstr + nonitem + "号"
							}
						})
						uni.showModal({
							title: '提示',
							content: `${nonstr}无房`,
						})
					} else {
						this.dateFlag = {};
						this.choice = true;
						this.dayCount = count + 1;
						this.dayCount2 = "共" + (count + 1) + "晚";
					}
				} else {
					var that = this;
					this.date.forEach(function(dataItems) {
						dataItems.forEach(function(dataItem) {
							dataItem.act.defaultStr = 0;
							if (dataItem.dateTime != that.date[index][indexs].dateTime) {
								dataItem.selected = 0;
								dataItem.act.tip = "";
							} else {
								dataItem.act.tip = "入住";

							}
						})
					})
					this.dateFlag = {
						date: this.date[index][indexs],
						index: index,
						indexs: indexs
					};
					this.choice = false;
					this.dayCount = 1;
					//
					this.choiceDate[0] = this.date[index][indexs];
				}
			},
			submitbtn: function() {
				this.choiceDate[0] = this.choiceDateArr[0];
				this.choiceDate[1] = this.choiceDateArr[this.choiceDateArr.length-1];
				this.dayCount2 = "共" + this.dayCount + "晚";
				this.hideCalendar();
			},
		},
	}
</script>

<style lang="scss">
	.layer-white-space {
		position: fixed;
		height: 100%;
		width: 100%;
		background-color: #ccc;
		opacity: .5;
	}

	.layer-header {
		height: 70upx;
		align-items: center;
		position: relative;

		.layer-close {
			width: 34upx;
			height: 34upx;
			position: absolute;

			left: 25upx;
			top: 50%;
			transform: translateY(-50%);

			//
			::before {
				content: "";
				width: 34upx;
				height: 1upx;
				background-color: #F93F4A;
				position: absolute;
				top: 16upx;
				left: 0;
				transform: rotate(45deg);
			}

			::after {
				content: "";
				width: 1upx;
				height: 34upx;
				background-color: #F93F4A;
				position: absolute;
				top: 0;
				left: 16upx;
				transform: rotate(45deg);
			}
		}

		.layer-title {
			text-align: center;
			flex: 1;
			font-size: 36upx;
		}
	}


	.calendar-layer {
		position: fixed;
		bottom: -100%;
		height: 0;
		width: 100%;
		overflow: hidden;
		z-index: 1111;
	}

	.layer-content {
		position: absolute;
		height: 85%;
		bottom: 0;
		font-size: 26upx;
		flex-direction: column;
		background-color: #FFF;
	}

	.layer-body {
		flex-direction: column;
		height: calc(100% - 70upx);
	}

	.layer-list {
		// position: absolute;
		// height:300upx;
		width: 100%;
		// bottom: 0upx;
		display: flex;
		flex: 1;
		flex-direction: column;
		position: relative;
	}

	.layer-footer {
		width: 100%;
		height: 120upx;
		align-items: center;
		justify-items: center;
		display: none;
	}

	.layer-content.choiceDate {
		.layer-footer {
			display: block;
		}

		.layer-body {
			height: calc(100% - 70upx - 120upx);
		}
	}

	.submitBtn {
		height: 80upx;
		line-height: 80upx;
		color: #FFF;
		border-radius: 10upx;
		margin: 20upx 50upx;
		justify-content: center;
		font-size: 16px;
		background: linear-gradient(to right, #F5504F, #F43F4F);
	}

	.month {
		width: 750upx;
		flex-direction: column;
		margin-bottom: 50upx;

		.month-title {
			font-size: 30upx;
			text-align: center;
			width: 100%;
			height: 60upx;
			line-height: 60upx;
			justify-content: center;
			border-bottom: 1upx solid #ddd;
			background: #FFF;
		}

		&.fixed {
			.month-title {
				position: fixed;
				top: calc(15% + 70upx + 60upx);
				z-index: 11
			}

			&:first-child {
				padding-top: 60upx;
			}
		}

		// 		&:first-child {
		// 			.month-title {
		// 				border: 0;
		// 			}
		// 		}
		&:last-child {
			margin-bottom: 0;
		}

		.month-content {
			flex-wrap: wrap;
		}
	}

	.day {
		width: calc(100%/7);
		text-align: center;
		height: 90upx;
		align-items: center;
		justify-content: center;
		position: relative;

		.day-content {
			flex-direction: column;
			line-height: 1;
		}

		.day-txt {
			width: 100%;
			height: 1.2em;
		}

		.day-tip {
			// margin-top: 5upx;
			font-size: 20upx;
			height: 1.2em;
		}

		.day-subject {
			// margin-top: 5upx;
			font-size: 14upx;
			height: 1.2em;
		}

		&.disabled {
			color: #ccc;
		}

		&.active {
			background: #FEEEEF;
		}

		.beginTip {
			display: none;
			width: 115upx;
			margin-top: -70upx;
			position: absolute;
			background: rgba(0, 0, 0, 0.6);
			border-radius: 5upx;
			text-align: center;
			padding: 6upx 10upx;
			font-size: 16upx;
			z-index: 33;

			&::after {
				content: "";
				position: absolute;
				left: 35%;
				top: 28upx;
				border: 5px solid transparent;
				border-top: 6px solid rgba($color: #000000, $alpha: 0.6);
			}
		}

		.endTip {
			display: none;
			background: rgba($color: #000000, $alpha: 0.6);
			position: absolute;
			border-radius: 5upx;
			text-align: center;
			padding: 6upx 10upx;
			// min-width: 70upx;
			margin-top: -70upx;
			font-size: 16upx;
			left: 12%;
			z-index: 33;

			&::after {
				content: "";
				position: absolute;
				left: 35%;
				top: 28upx;
				border: 5px solid transparent;
				border-top: 6px solid rgba($color: #000000, $alpha: 0.6);
			}
		}

		&.begin,
		&.end {
			background: #F93F4A;
			color: #FFF;
		}

		&.begin {
			border-radius: 8upx 0 0 8upx;

			.beginTip {
				display: block;
			}
		}

		&.end {
			border-radius: 0 8upx 8upx 0;

			.endTip {
				display: block;
			}
		}
	}


	/* ///////////////////////////////////////// */
	.week-box {
		height: 60upx;
		line-height: 60upx;
		border-bottom: 1upx solid #ddd;
		background: #FFF;
		position: relative;
		z-index: 11;
	}

	.week-box-item {
		width: calc(100%/7);
		text-align: center;

		&:first-child,
		&:last-child,
			{
			color: #F93F4A;
		}
	}

	/* ///////////////////order-time////////////////////// */
	.order-time {
		position: relative;
		width: 100%;
		padding: 25upx 15upx;
	}

	.order-time::after {
		position: absolute;
		content: "";
		width: 100%;
		bottom: 0;
		left: 0;
		border-top: 1px solid #eee;
		transform-origin: 0 100%;
		transform: scaleY(0.5);
	}

	.goInHotel {
		margin-left: 20upx;
		font-size: 26upx;
		color: gray;
	}

	.goInHotel2 {
		font-size: 26upx;
		color: gray;
	}

	.date-wrappper {
		position: relative;
		padding: 0 10upx;
		font-size: 32upx;
		color: black;
	}

	.left-hotel {
		margin-left: 40upx;
		font-size: 26upx;
		color: gray;
	}

	.order-time .sumCount {
		color: #6495ed;
		float: right;
		margin-right: 15upx;
		font-size: 26upx;
		padding-top: 10upx;
	}
</style>
