<template>
	<!-- 水印 -->
	<view class="watermark">
		<block v-for="(item,index) in num" :key="index">
			<view class="watermark-text" v-if="text != ''" :style="{opacity:opacity}">{{text}}</view>
			<image class="watermark-img" :src="imgUrl" mode="aspectFill" v-if="imgUrl !='' && text ==''" :style="{opacity:opacity}"></image>
		</block>
	</view>
</template>

<script>
	export default {
		name:'wm-watermark',
		props:{
			text:{			//设置水印文字
				type:String,
				default:''
			},
			imgUrl:{		//设置水印图片
				type:String,
				default:''
			},
			opacity:{		//设置透明度
				type:[Number,String],		
				default:0.6
			},
			num:{			//设置水印数量
				type:Number,
				default:20
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.watermark{
	position: fixed;
	width: 100%;
	height: 100%;
	top:0;
	left: 0;
	bottom: 0;
	right: 0;
	pointer-events: none;
	z-index: 9999999;
	display: flex;
	flex-wrap: wrap;
	overflow: hidden;
	.watermark-text{
		width: 375upx;
		height: 375upx;
		display: flex;
		justify-content: center;
		align-items: center;
		transform: rotate(-45deg);
		color: #ffffff;
		font-size: 32upx;
		white-space: nowrap;
	}
	.watermark-img{
		width: 375upx;
		height: 375upx;
		z-index: 1;
	}
}
</style>
