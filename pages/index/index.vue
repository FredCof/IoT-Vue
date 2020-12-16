<template>
	<view class="device-list">
		<uni-search-bar></uni-search-bar>
		<view class="nodivice" v-if="devlist.length == 0">
			<image src="../../static/backgroud.png" mode="widthFix"></image>
		</view>
		<uni-swipe-action>
			<uni-swipe-action-item v-for="item in devlist" :key="item.devid">
			    <device-info :devdata="item"></device-info>
				<template v-slot:right>
				    <view class="delete-btn" @tap="onDelete(item.devid)">
						<image src="../../static/Garbage.png" mode="widthFix"></image>
					</view>
				</template>
			</uni-swipe-action-item>
		</uni-swipe-action>
		<view class="add-button" @tap="scanDev">
			<image src="../../static/Plus.png"></image>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
			this.loadDivice();
		},
		data() {
			return {
				options:[
				        {
				            text: '取消',
				            style: {
				                backgroundColor: '#007aff'
				            }
				        }, {
				            text: '确认',
				            style: {
				                backgroundColor: '#dd524d'
				            }
				        }
				      ],
				imei: "",
				devid: "",
				devlist: []
			};
		},
		onPullDownRefresh() {
			return;
		},
		methods:{
			onDelete(rmdid){
				console.log('点击了按钮'+rmdid)
				for (let item in this.devlist){
					if (this.devlist[item].devid == rmdid){
						this.devlist.splice(item,1);
					}
				}
				uni.setStorageSync("devlist", this.devlist);
			},
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
								let flag = true;
								for (let item in this.devlist){
									if (this.devlist[item].devid == this.devid){
										flag = false;
										break;
									}
								}
								if (flag){
									this.devlist.push({
										devid: res.data.deviceId,
										name: res.data.deviceName,
										setting: {
											tMax: 25,
											tMin: 3,
											hMax: 60,
											hMin: 10,
											iMax: 70,
											iMin: 0,
											alarm: true
										},
										info: {
											t: 0,
											h: 0,
											l: 0
										}
									});
								} else {
									errmsg = "设备已经注册";
								}
								uni.setStorageSync("devlist", this.devlist);
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
				uni.scanCode({//启动摄像头扫描模组二维码获取IMEI号
					success: (res) => {
						var result;
						console.log('条码类型：' + res.scanType);
						console.log('条码内容：' + res.result);
						if(res.result.length>15){
							result = res.result.substring(0,15);
						}else{
							result = res.result;
						}
						console.log('result：' + result);
						this.imei = result;
						this.regDev();
					},
					fail: (err) => {
						console.log(err);
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = res.authSetting['scope.camera'];
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: this.i18n.content_note.text_app_name+'需要使用您的相机，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				});
				// #endif
				// #ifdef H5
				this.imei = '867726033517824'
				this.regDev();
				// #endif
				
			},
			loadDivice(){
				let dl = uni.getStorageSync("devlist");
				console.log(dl);
				if ('' != dl){
					this.devlist = dl;
				}
			}
		}
	}
</script>

<style lang="scss">
	.nodivice{
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	.device-list{
		display: flex;
		flex-direction: column;
		padding:0;
	}
	.add-button{
		height: 80rpx;
		width: 80rpx;
		align-content: center;
		background-color: #ffffff;
		position: fixed;
		bottom: 40rpx;
		// #ifdef H5
		bottom: 140rpx;
		// #endif
		right: 40rpx;
		padding: 10rpx;
		border: none;
		border-radius: 1.5rem ;
		-webkit-box-shadow: 0 0 30rpx 2rpx rgba(149, 165, 154, 0.6);
		box-shadow: 0 0 30rpx 5rpx rgba(149, 165, 154, 0.6);
		image{
			width: 80rpx;
			height: 80rpx;
		}
	}
	.loading{
		z-index: 100;
	}
	.delete-btn{
		margin: 20rpx;
		width: 200rpx;
		border-radius: 2rem 5rem 5rem 2rem;
		-webkit-box-shadow: 0 0 30rpx 0 rgba(255, 172, 174, 0.8);
		box-shadow: 0 0 30rpx 0 rgba(255, 75, 78, 0.8);
		image{
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			width: 80rpx;
		}
	}
</style>
