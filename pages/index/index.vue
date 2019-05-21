<template>
	<view class="content"><calendar @change="change"></calendar></view>
</template>

<script>
import calendar from '../../components/date-picker/date-picker.vue';

export default {
	data() {
		return {
			beginDate: '2019-05-06',
			endDate: '2019-05-07'
		};
	},
	components: {
		calendar
	},
	onLoad() {},
	methods: {
		change({ choiceDate, dayCount }) {
			//参数解释
			//1.choiceDate 时间区间（开始时间和结束时间）
			//2.dayCount 共多少晚

			console.log(choiceDate, dayCount);
			console.log('入住从 ' + choiceDate[0].re + '  到 ' + choiceDate[1].re + '  共 ' + dayCount + ' 晚');
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
}

.sliderBox {
	justify-content: center;
	margin-right: 50upx;
}

.text-center {
	justify-content: center;
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
</style>
