<template>
	<view>
		<view class="addCartMain">
			<view class="addw">
				<wm-number-box :min="0" :max="10" :initStep="3" destination="addCart" Shopimg="/static/demo/1.jpg"></wm-number-box>
			</view>
			<view class="addw">
				<wm-number-box :min="0" :max="10" destination="addCart" Shopimg="/static/demo/2.jpg"></wm-number-box>
			</view>
			<view class="addw">
				<wm-number-box :min="0" :max="10" destination="addCart2" Shopimg="/static/demo/3.jpg"></wm-number-box>
			</view>
			<view class="addw">
				<wm-number-box :min="0" :max="10" destination="addCart1"></wm-number-box>
			</view>
			<view class="addw">
				<wm-number-box :min="0" :max="10" destination="addCart2"></wm-number-box>
			</view>
			<view class="addw">
				<wm-number-box :min="0" :max="10" destination="addCart"></wm-number-box>
			</view>
		</view>
		<view class="page-bot">
			<view id="addCart">加入购物车1</view>
			<view id="addCart1">加入购物车2</view>
			<view id="addCart2">加入购物车3</view>
		</view>
	</view>
</template>
<!-- js -->
<script>
	import wmNumberBox from '../../components/wm-number-box/wm-number-box.vue'
var _this;
export default {
	components: {
		/* 组件注册 */
		wmNumberBox
	},
	data() {
		return {};
	},
	/* 页面加载 */
	onLoad(e) {
		_this = this;
	},
	/* 对象定义 */
	methods: {}
};
</script>
<!-- css -->
<style lang="scss">
	.page-bot{
		position: fixed;
		bottom: 0;
		height: 100rpx;
		background-color: #f1f1f1;
		width: 100%;
		display: flex;
		justify-content: space-between;
		font-size: 26rpx;
		view{
			flex: 1;
			display: flex;
			align-items: center;
			display: flex;
			justify-content: center;
		}
		view:last-child{
			background-color: #e41d1a;
			color: #ffffff;
		}
	}
	
	
	
	
	
	
	
	
	
	
	.addCartMain{
		display: flex;
		flex-direction: column;
	}
	.addw{
		width: 50%;
		height: 100rpx;
		display: flex;
		align-items: flex-end;
		justify-content: center;
	}
</style>
