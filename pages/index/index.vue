<template>
	<view class="content">
		<calendar></calendar>
	</view>
</template>

<script>
	import calendar from '../../components/date-picker/date-picker.vue';

	export default {
		data() {
			return {}
		},
		components: {
			calendar
		},
		onLoad() {

		},
		methods: {
		}
	}
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
