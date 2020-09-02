<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	.logo{
		width: 223rpx;
		height: 69rpx;
		background: url(static/logo.png) no-repeat center center;
		background-size: cover;
		position: absolute;
		right: 29rpx;
		top: 38rpx;
	}
	.pic{
		width: 100%;
	}
	
	.bg1{
		position: absolute;
		width: 100%;
		height: auto;
		z-index: -1;
		top: 0;
	}
	.bg2{
		position: absolute;
		width: 100%;
		height: auto;
		z-index: -1;
		bottom: 0rpx;
	}
</style>
