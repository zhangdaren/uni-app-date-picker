<template>
	<view class="content">
		<text class="text-center title">酒店日期选择插件</text>

		<!-- 组件模式(有界面)  -->
		<text class="text-center subTitle">1.组件模式(有界面)</text>
		<calendar @change="change" :startDate="initStartDate" :endDate="initEndDate" :daysCount="daysCount"></calendar>
		<button class="btn" @tap="setRandomDate()">随机设置日期</button>

		<!-- 组件模式(有界面)  -->
		<text class="text-center subTitle">2.组件模式(有界面)-->单个日期选择</text>
		<calendar @change="change_single" startDate="2019-11-07" :daysCount="daysCount" :singleDate="singleDate"></calendar>


		<!-- 弹窗模式(无界面) -->
		<text class="text-center subTitle">3.弹窗模式(无界面)</text>
		<text class="text-center desc">需要设置modal为true(默认为false), 通过show来控制隐藏还是显示</text>
		<calendar @change="change2" :modal="true" :show="showCaledar"></calendar>
		<text class="text-center dateStr">{{ dateStr }}</text>
		<button class="btn" @tap="showCaledar = !showCaledar">显示/隐藏{{showCaledar}}</button>
		
	</view>
</template>

<script>
	import calendar from '../../components/date-picker/date-picker';

	export default {
		data() {
			return {
				showCaledar: false,
				dateStr: '',
				daysCount: 130,
				singleDate: true,

				//初始日期
				initStartDate: '2019-12-06',
				initEndDate: '2019-12-07',
			};
		},
		components: {
			calendar
		},
		onLoad() {},
		onBackPress() {
			if (this.showCaledar !== false) {
				this.showCaledar = false;
				return true;
			}
		},
		methods: {
			change({
				choiceDate,
				dayCount
			}) {
				//参数解释
				//1.choiceDate 时间区间（开始时间和结束时间）
				//2.dayCount 共多少晚

				// console.log(choiceDate, dayCount);
				console.log('入住从 ' + choiceDate[0].re + '  到 ' + choiceDate[1].re + '  共 ' + dayCount + ' 晚');
			},
			change2({
				choiceDate,
				dayCount
			}) {
				this.dateStr =
					'入住从 ' + choiceDate[0].re + ' (星期' + choiceDate[0].week + ')  到 ' + choiceDate[1].re + '(星期' + choiceDate[1].week +
					')' + '  共 ' + dayCount + ' 晚 ';
			},
			change_single({
				choiceDate,
				dayCount
			}) {
				console.log(choiceDate, dayCount);
			},
			setRandomDate() {
				this.initStartDate = '2019-12-' + parseInt(Math.random() * 29 + 1);
				this.initEndDate = '2019-12-31';
			}
		}
	};
</script>

<style>
	view {
		display: flex;
	}

	.content {
		justify-content: center;
		flex-direction: column;
		padding: 0 20upx;
	}

	.sliderBox {
		justify-content: center;
		margin-right: 50upx;
	}

	.text-center {
		justify-content: center;
	}

	.title {
		font-size: 48upx;
		/* font-weight: bold; */
		text-align: center;
		margin-top: 50upx;
	}

	.subTitle {
		margin-top: 70upx;
		margin-bottom: 30upx;
		font-weight: bold;
	}

	.desc {
		color: #999999;
		font-size: 30upx;
	}

	.dateStr {
		font-size: 32upx;
		margin-top: 30upx;
	}

	.rowBox {
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.mrg50T {
		margin-top: 50upx;
	}

	.tips {
		color: #999;
		font-size: 24upx;
		text-align: center;
		margin-top: 100upx;
	}

	.btn {
		margin-top: 50upx;
	}
</style>
