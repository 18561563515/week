<template>
	<view class="content">
		<view class="logo"></view>
		
		<view class="bg1">
			<image src="../../static/bg1.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="bg2">
			<image src="../../static/bg2.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="title">
			<image src="../../static/title.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="title1">
			<image src="../../static/ganxie.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="btn" @click="linkweb">
			<image src="../../static/btnLink.png" mode="widthFix" class="pic"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url:'https://www.yammer.com/'
			}
		},
		methods: {
			linkweb(){
				
			}
		},
		onShow() {
			setTimeout(()=>{
				location.href = this.url
			},3000)
		}
	}
</script>

<style>
	page {
		background: #e9edef;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		position: absolute;
		left: 0;
		bottom: 0;
		right: 0;
		top: 0;
		/* height: 100%; */
	}
.title {
		width: 684rpx;
		height: 111rpx;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 135rpx;
	}
.title1 {
		width: 674rpx;
		height: 304rpx;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 340rpx;
	}
	.btn{
		width: 608rpx;
		height: 125rpx;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 705rpx;
	}
</style>
