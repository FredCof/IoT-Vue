<template>
	<view class="control-page">
		<view class="header">
			<image src="../../static/pico.jpg"></image>
		</view>
		<view class="des-card">
			<view class="setline">
				<text class="des-text">设定温度范围：</text>
				<view>
					<uni-number-box :max="40" :min="0" :value="minTemp" :control="false" @change="TI"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="40" :min="0" :value="maxTemp" :control="false" @change="TA"></uni-number-box>
				</view>
				<button class="sendbtn" @tap="changeMonitor(4)">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">设定湿度范围：</text>
				<view>
					<uni-number-box :max="99" :min="0" :value="minHud" :control="false" @change="HI"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="99" :min="0" :value="maxHud" :control="false" @change="HA"></uni-number-box>
				</view>
				<button class="sendbtn" @tap="changeMonitor(5)">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">设定光强范围：</text>
				<view>
					<uni-number-box :max="99" :min="0" :value="minIns" :control="false" @change="II"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="99" :min="0" :value="maxIns" :control="false" @change="IA"></uni-number-box>
				</view>
				<button class="sendbtn" @tap="changeMonitor(6)">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">开启监控</text>
				<switch :checked="alert" @change="changePermissions" :color="'#4dc25f'" :disabled="false"/>
			</view>
			<view class="setline">
				<text class="des-text">下发权限</text>
				<button class="sendbtn" @tap="sendAuth">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				alert: true,
				maxTemp: 0,
				minTemp: 0,
				maxIns: 0,
				minIns: 0,
				maxHud: 0,
				minHud: 0,
			}
		},
		mounted() {
			let setting = uni.getStorageSync("device").setting;
			this.maxTemp = setting.tMax;
			this.minTemp = setting.tMin;
			this.maxHud = setting.hMax;
			this.minHud = setting.hMin;
			this.maxIns = setting.iMax;
			this.minIns = setting.iMin;
			this.alert = setting.alarm;
		},
		methods: {
			TA(val){
				this.maxTemp = val;
			},
			TI(val){
				this.minTemp = val;
			},
			HA(val){
				this.maxHud = val;
			},
			HI(val){
				this.minHud = val;
			},
			IA(val){
				this.maxIns = val;
			},
			II(val){
				this.minIns = val;
			},
			changePermissions(val){
				this.alert = !this.alert;
				if (this.alert)
				{
					this.sendCmd("", 2);
				} else{
					this.sendCmd("", 3)
				}
			},
			sendAuth(){
				this.sendCmd("", 1);
			},
			sendCmd(paracmdstr, paracmdcode){
				console.log("sendCmd");
				//{"cmdstring":"{"L1":0,"L2":0}","cmdlen":15,"cmdcode":3}
				let cmdpara = {
					cmdstring:paracmdstr,
					cmdlen:paracmdstr.length,
					cmdcode:paracmdcode
				}
				let cmdstr = JSON.stringify(cmdpara);
				console.log("cmdstr:"+cmdstr);
				
				uni.request({
					url: this.globalVal.default_url.devCmd,
					method: 'POST',
					data: {
						deviceId:this.devid,
						cmdInfo:cmdstr
					},
					success: res => {
						uni.showToast({//向云端服务发送命令下发请求
							title: '命令下发成功!请检查设备端',
							icon:"none",
							duration:1000
						});
					},
					fail: () => {
						uni.showToast({//向云端服务发送命令下发请求
							title: '命令下发失败!请检查设备端',
							icon:"none",
							duration:1000
						});
					},
					complete: () => {
					}
				});
				this.saveSetting();
			},
			saveSetting(){
				let devlist = uni.getStorageSync("devlist");
				let device = uni.getStorageSync("device");
				for (let dev in devlist) {
					if (devlist[dev].devid == this.devid){
						devlist[dev].setting.tMax = this.maxTemp;
						devlist[dev].setting.tMin = this.minTemp;
						devlist[dev].setting.hMax = this.maxHud;
						devlist[dev].setting.hMin = this.minHud;
						devlist[dev].setting.iMax = this.maxIns;
						devlist[dev].setting.iMin = this.minIns;
						devlist[dev].setting.alarm = this.alert;
						break;
					}
				}
				device.setting.tMax = this.maxTemp;
				device.setting.tMin = this.minTemp;
				device.setting.hMax = this.maxHud;
				device.setting.hMin = this.minHud;
				device.setting.iMax = this.maxIns;
				device.setting.iMin = this.minIns;
				device.setting.alarm = this.alert;
				uni.setStorageSync("device", device);
				uni.setStorageSync("devlist", devlist);
			},
			changeMonitor(code){
				let max = 0, min = 0;
				switch (code){
					case 4:
						max = this.maxTemp, min = this.minTemp;
						break;
					case 5:
						max = this.maxHud, min = this.minHud;
						break;
					case 6:
						max = this.maxIns, min = this.minIns;
						break;
				}
				this.sendCmd(min.toString().padStart(2, '0')+max.toString().padStart(2, '0'), code);
			}
		},
		computed:{
			devid(){
				return uni.getStorageSync("device").devid
			}
		}
	}
</script>

<style lang="scss">
	.control-page{
		display: flex;
		flex-direction: column;
		justify-content:center;
	}
	.header {
		box-shadow:0rpx 0rpx 60rpx 0rpx rgba(0,0,0,0.1);
		overflow: hidden;
		width: 400rpx;
		height: 400rpx;
		border-radius:50%;
		margin: 100rpx auto 40rpx;
		position: relative;
		image{
			width: 400rpx;
		}
	}
	.des-card{
		background-color: #fcfcfd;
		width: 90%;
		margin: 0 5%;
		border-radius: 1rem;
		padding: 30rpx 0;
		-webkit-box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
		box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
	}
	.setline{
		display: flex;
		margin: 20rpx 20rpx;
		font-size: 35rpx;
		.des-text{
			align-self: center;
		}
		.des-blod{
			font-size: 40rpx;
			font-weight: 900;
		}
		switch{
			display: inline-block;
			margin-left: auto;
			margin-right: 0;
			transform: translateX(20rpx) scale(0.8);
		}
	}
	.sendbtn{
		padding: 0;
		display: inline-flex;
		flex-direction: row;
		border-radius: 10rem;
		border: none !important;
		outline: none !important;
		box-shadow:0rpx 0rpx 20rpx 2rpx rgba(164,217,228,0.8);
		margin-left: auto;
		margin-right: 0;
		image{
			margin: 10rpx;
			height: 60rpx;
		}
		&::after{
			border: none;
		}
	}
</style>
