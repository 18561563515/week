<template>
	<view class="content">
		<view class="mark" @click="hideMark" v-if="hidemark">
			<image :src="src" mode="widthFix" class="pic"></image>
		</view>
		<view class="logo"></view>
		
		<!-- <view class="bg1">
			<image src="../../static/bg1.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="bg2">
			<image src="../../static/bg2.png" mode="widthFix" class="pic"></image>
		</view> -->
		<view class="title">
			<image src="../../static/title.png" mode="widthFix" class="pic"></image>
		</view>
		<view class="rongqi">
			<view class="other">
				<picker @change="bindPickerChange" :value="index" :range="array" mode="selector" class="picker">
			
				</picker>
				<view class="sanjiao">
					
				</view>
				<view class="uni-input">{{array[index]}}</view>
			</view>
			<view class="rongqi1">
				<view class="box" v-for="(i,index) in imgarr" :key="index" @click="clickPic(i.url)">
					<image :src="i.url" mode="aspectFill" class="pic"></image>
					<!-- <easy-loadimage mode="widthFix"
					                :scroll-top="scrollTop"
					                :image-src="i.url"></easy-loadimage> -->
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	import easyLoadimage from '@/components/easy-loadimage/easy-loadimage.vue'
	export default {
		data() {
			return {
				array: ['中国', '美国', '巴西', '日本', '韩国', '俄罗斯', '朝鲜'],
				index: 0,
				imgarr:[],
				scrollTop:0,
				hidemark:false,
				src:''
			}
		},
		components:{
			easyLoadimage
		},
			
		onShow() {
			this.init()
		},
		methods: {
			onPageScroll({scrollTop}) {
			            // 传入scrollTop值并触发所有easy-loadimage组件下的滚动监听事件
			            this.scrollTop = scrollTop;
			        },
			bindPickerChange(e) {
				console.log('大区值为', e.target.value)
				this.index = e.target.value
				this.init()
			},
			clickPic(e){
				console.log(e)
				this.hidemark = !this.hidemark
				this.src = e
			},
			init(){
				uni.showLoading({
					title:'加载中···',
					mask:true
				})
				uni.request({
					url:'http://3w.donglianguoji.com/app/week/php/txt.php',
					method:'POST',
					data:{index:this.index},
					header:{'content-type': 'application/x-www-form-urlencoded'},
					success: (e) => {
						uni.hideLoading()
						console.log(e)
						this.imgarr = e.data
						if(e.data.length == 0){
							uni.showModal({
								content:'该分类下暂无照片',
								showCancel:false
							})
						}
					},
					fail: (m) => {
						console.log(m)
					}
				})
			},
			
			hideMark(){
				this.hidemark = !this.hidemark
			}
		}
	}
</script>

<style>
page {
		/* background: #e9edef; */
		background: url(../../static/bg.jpg) no-repeat bottom center;
		background-size: 100% auto;
	}
.mark{
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0,0,0,.7);
	z-index: 999;
	display: flex;
	flex-direction: row;
	justify-content: center;
	align-items: center;
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
	
	.rongqi{
		width: 664rpx;
		height: 836rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #fff;
		position: absolute;
		top: 314rpx;
		left: 50%;
		transform: translateX(-50%);
	}
	.rongqi1{
		/* width: calc(100% - 60rpx);
		padding: 0 30rpx; */
		height: 806rpx;
		overflow-y: scroll;
		
		border-radius: 10rpx;
	}
	.box{
		float: left;
		width: 270rpx;
		height: 208rpx;
		background: #E9EDEF;
		margin: 30rpx;
		overflow: hidden;
		border-radius: 15rpx;
	}
	.other {
		height: 80rpx;
		width: 100%;
		position: relative;
		/* border: 1px solid #007AFF; */
	}
	.picker{
		height: 100%;
		width: 100%;
	}
	.uni-input {
		font-size: 26rpx;
		height: 100%;
		line-height: 80rpx;
		position: absolute;
		/* width: 100%; */
		left: 0;
		top: 0;
		z-index: -1;
		display: flex;
		flex-direction: row;
		align-items: center;
		padding-left: 40rpx;
	}
	.sanjiao{
		width: 0;
		height: 0;
		border-top: 20rpx solid #000;
		border-right: 20rpx solid transparent;
		position: absolute;
		left: 15rpx;
		top: 15rpx;
	}
	.pic1{
		height: 100%;
	}
	/* .rongqi1{display: flex;justify-content: space-between;flex-wrap: wrap;padding: 32rpx;background: #F1F1F1;} */
	   /* .rongqi1 .item{width: 48%;background: #fff;margin-bottom: 80rpx;border-radius: 20rpx;}
	    .rongqi1 .item >>> .easy-loadimage{
	        width: 100%;
	        margin-bottom: 38rpx;
	    }
	    .rongqi1 .item >>> .origin-img{
	        border-radius: 20rpx;
	    }
	   
	    .rongqi1 .item >>> .loadfail-img,.rongqi1 .item >>>.loading-img{
	        height: 500rpx;
	    } */
</style>
