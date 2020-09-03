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
		<view v-if="choosePhoto">
			<view class="photo-box" @click="upload">

			</view>
			<view class="seephoto"></view>
		</view>
		<view v-else>
			<view class="photo-content">
				<image :src="form.src" mode="aspectFill" class="photo-content-1"></image>
			</view>
			<text class="txt">#SMILE WITH SURGICEL</text>
			<view class="form-box">
				<view class="form-box-1">
					<view class="txt-pic">
						<image src="../../static/txt1.png" mode="widthFix" class="txt-pic-1"></image>
					</view>
					<view class="other">
						<input type="text" value="" class="name-input" v-model="form.name" />
					</view>
				</view>
				<view class="form-box-1">
					<view class="txt-pic">
						<image src="../../static/txt2.png" mode="widthFix" class="txt-pic-2"></image>
					</view>
					<view class="other">
						<input type="text" value="" class="name-input" v-model="form.hospital" />
					</view>
				</view>
				<view class="form-box-1">
					<view class="txt-pic">
						<image src="../../static/txt3.png" mode="widthFix" class="txt-pic-3"></image>
					</view>
					<view class="other">
						<picker @change="bindPickerChange" :value="index" :range="array" mode="selector" class="picker">

						</picker>
						<view class="uni-input">{{array[index]}}</view>
					</view>
				</view>

				<view class="form-box-1">
					<view class="txt-pic">
						<image src="../../static/txt4.png" mode="widthFix" class="txt-pic-4"></image>
					</view>
					<view class="other">
						<picker @change="bindPickerChange1" :value="index1" :range="array0" mode="selector" class="picker">

						</picker>
						<view class="uni-input">{{array0[index1]}}</view>
					</view>
				</view>
				<view class="form-box-1">
					<view class="txt-pic">
						<image src="../../static/txt5.png" mode="widthFix" class="txt-pic-5"></image>
					</view>
					<view class="other">
						<input type="text" value="" class="name-input" v-model="form.address" />
					</view>
				</view>

			</view>
			<view class="upload-btn">
				<view class="xinxi" v-if="maskshow">
					<text>正在上传照片：{{jindu}}%</text>
					<text>请勿关闭页面</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				choosePhoto: true,
				maskshow:false,
				jindu:0,
				form: {
					name: '',
					src: '',
					hospital: '',
					address: '',
					area1: '',
					area2: ''
				},
				array: ['中国', '美国', '巴西', '日本', '韩国', '俄罗斯', '朝鲜'],
				array0: [],
				index: 0,
				index1: 0,
				array1: [
					['中国1', '美国1', '巴西1'],
					['中国2', '美国2', '巴西2'],
					['中国3', '美国3', '巴西3'],
					['中国4', '美国4', '巴西4'],
					['中国5', '美国5', '巴西5'],
					['中国6', '美国6', '巴西6'],
					['中国7', '美国7', '巴西7'],
				]

			}
		},
		onLoad() {

		},
		methods: {
			upload() {
				let that = this
				var shijian = new Date().getTime()
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						console.log(res);
						this.choosePhoto = false
						this.form.src = res.tempFilePaths[0]
						if (res.size > 4000000) {
							uni.showModal({
								content: '照片最大4M',
								showCancel: true
							});
						} else {
							that.maskshow = !that.maskshow
							let obj = Object.assign(this.form, {
								shijian: shijian
							})
							console.log('obj', obj)
							const uploadTask = uni.uploadFile({
								url: 'http://3w.donglianguoji.com/app/week/php/shipin.php', //仅为示例，非真实的接口地址
								filePath: this.form.src,
								name: 'file',
								formData: obj,
								success: (uploadFileRes) => {
									console.log(uploadFileRes.data);
								}
							});
							uploadTask.onProgressUpdate((res) => {
								console.log('上传进度' + res.progress);
								that.jindu = res.progress
								// console.log('已经上传的数据长度' + res.totalBytesSent);
								// console.log('预期需要上传的数据总长度' + res.totalBytesExpectedToSend);
								if (res.progress >= 100) {
									// uni.hideLoading()
									uni.redirectTo({
										url: '../chenggong/chenggong'
									})
								}

							});
						}
					}
				});
			},
			bindPickerChange(e) {
				console.log('大区值为', e.target.value)
				this.index = e.target.value
				this.form.area1 = e.target.value
				this.array0 = this.array1[this.index]
				console.log(this.array0)
			},
			bindPickerChange1(e) {
				console.log('小区值为', e.target.value)
				this.index1 = e.target.value
				this.form.area2 = e.target.value
			},
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


	.pic {
		width: 100%;
		display: block;
	}

	.title {
		width: 684rpx;
		height: 111rpx;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 135rpx;
	}

	.photo-box {
		width: 461rpx;
		height: 431rpx;
		background: url(../../static/photo.png) no-repeat center center;
		background-size: 100%;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 311rpx;
	}

	.seephoto {
		width: 493rpx;
		height: 140rpx;
		background: url(../../static/seephoto.png) no-repeat center center;
		background-size: 100%;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 795rpx;
	}

	.photo-content {
		width: 580rpx;
		height: 396rpx;
		background: #fff;
		border-radius: 10rpx;
		box-shadow: 1px 1px 4px rgba(0, 0, 0, .3);
		position: absolute;
		top: 275rpx;
		left: 50%;
		transform: translateX(-50%);
		overflow: hidden;
	}

	.txt {
		position: absolute;
		right: 87rpx;
		top: 684rpx;
		color: #c5161e;
		font-size: 20rpx;
	}

	.form-box {
		width: 572rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		position: absolute;
		top: 726rpx;
		left: 50%;
		transform: translateX(-50%);
	}

	.form-box-1 {
		width: 572rpx;
		height: 68rpx;
		margin-bottom: 30rpx;
		border: 1px solid #007AFF;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}

	.txt-pic {
		width: 142rpx;
		height: 30rpx;
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.txt-pic-1 {
		width: 138rpx;
		height: 29rpx;
	}

	.txt-pic-2 {
		width: 142rpx;
		height: 30rpx;
	}

	.txt-pic-3 {
		width: 139rpx;
		height: 29rpx;
	}

	.txt-pic-4 {
		width: 139rpx;
		height: 29rpx;
	}

	.txt-pic-5 {
		width: 141rpx;
		height: 28rpx;
	}

	.other {
		flex: 1;
		height: 100%;
		position: relative;
	}

	.name-input {
		height: 100%;
		padding-left: 20rpx;
		font-size: 26rpx;
	}

	.uni-input {
		font-size: 26rpx;
		height: 100%;
		line-height: 30rpx;
		position: absolute;
		/* width: 100%; */
		left: 0;
		top: 0;
		z-index: -1;
		display: flex;
		flex-direction: row;
		align-items: center;
		padding-left: 20rpx;
	}

	.picker {
		height: 100%;
	}

	.photo-content-1 {
		width: 100%;
		height: 100%;
	}

	.upload-btn {
		width: 300rpx;
		height: 100rpx;
		background: #808080;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 1220rpx;
	}
	.xinxi{padding: 25rpx 40rpx;display: flex;flex-direction: column;justify-content: center;align-items: center;background: #fff;border-radius: 20rpx;}
	.xinxi>text{line-height: 60rpx;color: #DD524D;}
</style>
