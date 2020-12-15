<template>
	<view class="device-list">
		<view class="" v-if="devlist.length == 0">当前没有设备</view>
		<scroll-view v-for="item in devlist" :key="item.devid">
			<device-info :devdata="item"></device-info>
		</scroll-view>
		<view class="add-button" @tap="scanDev">
			<image src="../../static/Plus.png"></image>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
		},
		data() {
			return {
				imei: "",
				devid: "",
				devlist: []
			};
		},
		methods:{
			regDev(){
				console.log("regDev URL:"+this.globalVal.default_url.devReg);
				uni.showLoading({
					title: '注册中...',
					mask: false
				});
				var errcode = 1;
				var errmsg = "失败";
				uni.request({//向云端服务发送设备注册请求
					url: this.globalVal.default_url.devReg,
					method: 'POST',
					data: {
						imei:this.imei
					},
					success: res => {
						console.log(res);
						if(200 == res.statusCode){
							errcode = res.data.code;
							if(0 == errcode){
								errmsg = "注册成功";
								this.devid = res.data.deviceId;
								this.devlist.push({
									devid: res.data.deviceId,
									name: res.data.deviceName
								});
								console.log(this.devlist);
							}else{
								errmsg = res.data.errmsg;
							}
						}
					},
					fail: () => {},
					complete: () => {
						uni.hideLoading();
						uni.showToast({
							title: errmsg,
							icon:"none",
							duration:2500
						});
					}
				});
			},
			scanDev(){
				uni.showLoading({
					title: '数据加载中...',
					mask: false
				});
			    // #ifndef H5
				// uni.scanCode({//启动摄像头扫描模组二维码获取IMEI号
				// 	success: (res) => {
				// 		var result;
				// 		console.log('条码类型：' + res.scanType);
				// 		console.log('条码内容：' + res.result);
				// 		if(res.result.length>15){
				// 			result = res.result.substring(0,15);
				// 		}else{
				// 			result = res.result;
				// 		}
				// 		console.log('result：' + result);
				// 		this.imei = result;
				// 		console.log(this.imei);
				// 		// this.btnAddDisable = false;
				// 	},
				// 	fail: (err) => {
				// 		console.log(err);
				// 		// #ifdef MP
				// 		uni.getSetting({
				// 			success: (res) => {
				// 				let authStatus = res.authSetting['scope.camera'];
				// 				if (!authStatus) {
				// 					uni.showModal({
				// 						title: '授权失败',
				// 						content: this.i18n.content_note.text_app_name+'需要使用您的相机，请在设置界面打开相关权限',
				// 						success: (res) => {
				// 							if (res.confirm) {
				// 								uni.openSetting()
				// 							}
				// 						}
				// 					})
				// 				}
				// 			}
				// 		})
				// 		// #endif
				// 	}
				// });
				// #endif
				this.imei = '867726033517824'
				this.regDev();
			},
		}
	}
</script>

<style lang="scss">
	.device-list{
		display: flex;
		flex-direction: column;
		padding:0 20rpx;
	}
	.add-button{
		height: 80rpx;
		width: 80rpx;
		align-content: center;
		background-color: #f3f3f3;
		position: fixed;
		bottom: 20rpx;
		right: 20rpx;
		padding: 10rpx;
		border: none;
		border-radius: 1.5rem ;
		-webkit-box-shadow: 0 0 30rpx 0 rgba(149, 165, 154, 0.6);
		box-shadow: 0 0 60rpx 0 rgba(149, 165, 154, 0.6);
		image{
			width: 80rpx;
			height: 80rpx;
		}
	}
	.loading{
		z-index: 100;
	}
</style>
