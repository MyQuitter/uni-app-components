<template>
	<view>
		<wm-poster imgSrc="../../static/banner/1000.jpg" QrSrc="../../static/QRcode-t.jpg" :Title="CanvasTitle" PriceTxt="99.99" OriginalTxt="199.99" @success="OnImg"></wm-poster>
		<view class="Poster_main" v-show="Type">
			<view class="Poster_btn" @tap="onBtn">生成图片</view>
		</view>
		<view class="Poster_img" v-if="InitImg !=''">
			<image :src="InitImg" mode="aspectFit"></image>
		</view>
	</view>
</template>

<script>
var _this, _img;
import wmPoster from '../../components/wm-poster/wm-poster.vue';
export default {
	data() {
		return {
			CanvasTitle: '此处是标题,问一个很严肃的问题quitter帅嘛？我觉得还是比较帅的。',
			InitImg: '',
			Type:false
		};
	},
	components: {
		wmPoster
	},
	onLoad() {
		_this = this;
		uni.showLoading({
			title:"生成中..."
		})
	},
	methods: {
		OnImg: function(e) {
			console.log(e);
			_img = e.tempFilePath;
			_this.Type = true;
			uni.hideLoading();
		},
		onBtn:function(){
			_this.InitImg = _img;
		}
	}
};
</script>

<style lang="scss">
.Poster_main {
	display: flex;
	justify-content: center;
	padding: 20upx;
	.Poster_btn {
		padding: 10upx 30upx;
		background-color: #e31d1a;
		font-size: 28upx;
		color: #ffffff;
		border-radius: 5upx;
		transform: translate(0, 0);
		&:active {
			transform: translate(1px, 1px);
			cursor: pointer;
		}
	}
}
.Poster_img{
	margin: 0 auto;
	display: flex;
	justify-content: center;
	width: 700upx;
	height: 700upx;
	box-sizing: border-box;
	border: 20upx solid #8f6248;
	border-radius: 10upx;
	box-shadow: 0 0 10upx 0 #f1f1f1;
	image{
		width: 100%;
		height: 100%;
	}
}
</style>
