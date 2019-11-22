<template>
	<view class="_scroll-body">
		<template v-if="refreshType">
			<view class="_scroll-redraw" v-if="redraw == 2">
				<view class="_scroll-redraw-circle"></view>
				<view class="_scroll-redraw-txt">刷新中...</view>
			</view>
			<view class="_scroll-preload" :style="{ height: height + 'px' }" v-else>
				<view v-if="redraw == 0">下拉刷新</view>
				<view v-if="redraw == 1">释放刷新</view>
			</view>
			<view class="_redraw-end" :style="{ top: Endheight }">刷新成功！</view>
		</template>
		<scroll-view
			scroll-y="true"
			class="_scroll-view"
			:style="{ transform: coverTransfrom, transition: coverTransition }"
			@touchstart="__Start"
			@touchmove="__Move"
			@touchend="__End"
			lower-threshold="150"
			@scrolltolower="__lower"
		>
			<view class="_scroll-main" :style="{ 'min-height': bodyHight }">
				<slot></slot>
				<template v-if="Boolean(loadType) && InitPage">
					<view v-if="loadType == 1" class="_scroll-view-more">上拉显示更多</view>
					<view v-if="loadType == 2" class="_scroll-view-more">
						<view class="_scroll-view-more-circle"></view>
						<view class="_scroll-view-more-txt">正在加载...</view>
					</view>
					<view v-if="loadType == 3" class="_scroll-view-more">没有更多数据了</view>
				</template>
				<view class="_scroll_null" v-if="!InitPage">
					<image class="_scroll_null_img" :src="nullImg" mode="aspectFit"></image>
					<view class="_scroll_null_txt">{{ nullTxt }}</view>
				</view>
				<wmMore></wmMore>
			</view>
		</scroll-view>
	</view>
</template>
<!-- js -->
<script>
import wmMore from '@/components/module/course/wm-more.vue';
var _this;
let _startY = 0,
	_moveY = 0,
	pageAtTop = true;
export default {
	components: {
		/* 组件注册 */
		wmMore
	},
	name: 'wm-scroll' /* 组件名称 */,
	props: {
		loadType: {
			//上拉加载默认关闭   传入值为 1 2 3
			type: [String, Number],
			default: 0
		},
		refreshType: {
			//开启下拉刷新  默认开启
			type: Boolean,
			default: false
		},
		nullImg: {
			type: String,
			default: '../../static/default.jpg'
		},
		nullTxt: {
			type: String,
			default: '没有更多数据...'
		}
	},
	data() {
		return {
			coverTransfrom: 'translateY(0px)',
			coverTransition: 'transform .1s linear',
			moving: false, //下拉刷新是否在移动
			redraw: 0,
			height: 0,
			Endheight: '-60rpx',
			bodyHight: "max-content"
		};
	},
	computed: {
		InitPage() {
			return Boolean(this.$slots.default);
		}
	},
	methods: {
		//开始触发
		__Start(e) {
			if (!this.refreshType) return;
			if (pageAtTop === false) return;
			_startY = e.touches[0].clientY;
		},
		//触发中
		__Move(e) {
			if (!this.refreshType) return;
			if (pageAtTop === false) return;
			_moveY = e.touches[0].clientY;
			//实际Y轴移动距离减去默认触发距离
			let moverDistabce = _moveY - _startY - 80;
			//当Y轴滑动距离小于0时
			if (moverDistabce < 0) {
				this.moving = false;
				pageAtTop = true;
				return;
			}
			//开始移动
			this.moving = true;
			//当Y轴滑动距离大于等于设定距离时
			if (moverDistabce >= 80) {
				moverDistabce = 80;
				this.moving = false;
				this.redraw = 1;
			}
			//当滑动距离大于0且小于设定距离时开启引导动画
			if (moverDistabce > 0 && moverDistabce < 80) {
				this.redraw = 0;
			}
			//设置引导动画高度
			this.height = moverDistabce;
			//设置容器Y轴偏移值
			this.coverTransfrom = `translateY(${moverDistabce}px)`;
		},
		//触发结束
		__End() {
			if (!this.refreshType) return;
			//当容器没有移动时
			this.height = 0;
			if (this.moving === false) {
				if (this.redraw == 1) {
					this.redraw = 2;
					pageAtTop = false;
					this.coverTransfrom = 'translateY(40px)';
					this.coverTransition = 'transform 0.3s cubic-bezier(.21,1.93,.53,.64)';
					let _this = this;
					//完整触发下拉刷新
					_this.$emit('Up');
				}
				return;
			} else {
				this.moving = false;
				this.coverTransfrom = 'translateY(0px)';
				this.coverTransition = 'transform 0.3s cubic-bezier(.21,1.93,.53,.64)';
			}
		},
		//刷新成功后调用
		UpEnd() {
			let _this = this;
			_this.coverTransfrom = 'translateY(0px)';
			_this.moving = false;
			pageAtTop = true;
			_this.redraw = 0;
			_this.Endheight = '0';
			setTimeout(function() {
				_this.Endheight = '-60rpx';
			}, 1000);
		},
		//上拉加载更多
		__lower(e) {
			if (!Boolean(this.loadType)) return;
			this.$emit('Down');
		}
	},
	created() {
		let _this = this;
		// #ifdef H5
		setTimeout(function() {
			let un = uni.createSelectorQuery().in(this);
			un.select('._scroll-body')
				.boundingClientRect(data => {
					_this.bodyHight = data.height + 'px';
				})
				.exec();
		}, 30);
		// #endif
	}
};
</script>
<!-- css -->
<style lang="scss">
@mixin _flexCenter {
	display: flex;
	justify-content: center;
	align-items: center;
}
._scroll-body {
	height: 100%;
	overflow: hidden;
	position: relative;
	._scroll-preload {
		position: absolute;
		width: 100%;
		@include _flexCenter;
		font-size: 24rpx;
		color: var(--style);
		overflow: hidden;
		height: 0;
	}
	._scroll-redraw {
		position: absolute;
		width: 100%;
		height: 80rpx;
		@include _flexCenter;
		font-size: 24rpx;

		._scroll-redraw-circle {
			width: 28rpx;
			height: 28rpx;
			border: 1px solid var(--style);
			border-radius: 100%;
			border-bottom-color: transparent;
			border-top-color: transparent;
			-webkit-animation: mescrollRotate 0.6s linear infinite;
			animation: mescrollRotate 0.6s linear infinite;
		}

		._scroll-redraw-txt {
			margin-left: 10rpx;
			color: var(--style);
		}
	}

	._redraw-end {
		background-color: var(--style);
		@include _flexCenter;
		color: var(--white);
		position: absolute;
		font-size: 24rpx;
		height: 60rpx;
		opacity: 0.8;
		width: 100%;
		z-index: 99;
		top: -60rpx;
		transition: top 0.3s cubic-bezier(0.21, 1.93, 0.53, 0.64);
	}
	._scroll-view {
		height: 100%;

		._scroll-main {
			position: relative;
			padding-bottom: 70rpx;
			height: 100%;
			box-sizing: border-box;
		}

		._scroll-main:before,
		._scroll-main:after {
			content: '';
			display: table;
			clear: both;
		}

		._scroll-view-more {
			width: 100%;
			height: 80rpx;
			@include _flexCenter;
			font-size: 24rpx;
			color: var(--gray);

			._scroll-view-more-circle {
				width: 28rpx;
				height: 28rpx;
				border: 2rpx solid var(--gray);
				border-radius: 100%;
				border-bottom-color: transparent;
				border-top-color: transparent;
				-webkit-animation: mescrollRotate 0.6s linear infinite;
				animation: mescrollRotate 0.6s linear infinite;
				position: relative;
			}

			._scroll-view-more-txt {
				margin-left: 10rpx;
				color: var(--gray);
			}
		}

		._scroll_null {
			flex: 1;
			flex-direction: column;
			position: absolute;
			bottom: 0;
			left: 0;
			right: 0;
			top: 0;
			@include _flexCenter;
			background-color: var(--white);

			._scroll_null_img {
				width: 400rpx;
				height: 400rpx;
			}
			._scroll_null_txt {
				font-size: 28rpx;
				color: var(--gray);
				margin-top: 20rpx;
			}
		}
	}
}

/* 旋转动画 */

@-webkit-keyframes mescrollRotate {
	0% {
		-webkit-transform: rotate(0deg);
	}

	100% {
		-webkit-transform: rotate(360deg);
	}
}

@keyframes mescrollRotate {
	0% {
		transform: rotate(0deg);
	}

	100% {
		transform: rotate(360deg);
	}
}
</style>
