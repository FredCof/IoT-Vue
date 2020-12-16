<template>
	<navigator animation-type="zoom-out" animation-duration="300" :url="url">
		<view class="device-card oBorder">
			<view class="artpic">
				<image src="../../static/pic.jpg" class="oBorder"></image>
			</view>
			<view class="device-info">
				<text class="title">{{ devdata.name }}</text>
				<hr></hr>
				<view class="info">
					<text class="tag">温度</text>
					<text class="value">{{ termperature }}℃</text>
				</view>
				<view class="info">
					<text class="tag">湿度</text>
					<text class="value">{{ humidity }}%</text>
				</view>
				<view class="info">
					<text class="tag">光强</text>
					<text class="value">{{ intensity }}Lux</text>
				</view>
			</view>
		</view>
	</navigator>
</template>

<script>
	export default {
		data() {
			return {
				termperature: 0,
				humidity: 0,
				intensity: 0
			};
		},
		props:{
			devdata: Object
		},
		mounted() {
			uni.request({//向云端服务发送请求获取设备最新信息
				url: this.globalVal.default_url.devInfo,
				method: 'POST',
				data: {
					deviceId:this.devdata.devid
				},
				success: res => {
					console.log(res);
					if(200 == res.statusCode && undefined == res.data.error_code){
						this.userinfo = res.data.services[1].data.infostring;
						let val = JSON.parse(this.userinfo);
						this.termperature = val.T;
						this.humidity = val.H;
						this.intensity = val.L;
					}
				},
				fail: () => {},
				complete: () => {
					let devlist = uni.getStorageSync("devlist");
					for (let dev in devlist) {
						if (devlist[dev].devid == this.devdata.devid){
							devlist[dev].info.t = this.termperature;
							devlist[dev].info.h = this.humidity;
							devlist[dev].info.l = this.intensity;
							break;
						}
					}
					uni.setStorageSync("devlist", devlist);
				}
			});
		},
		methods:{},
		computed:{
			url(){
				return '../../pages/device/devindex?device='+this.devdata.devid
			}
		}
	}
</script>

<style lang="scss">
	$picpad: 10rpx;
	.device-card{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		color: #333333;
		width: calc(100% - 80rpx);
		width: 100%;
		padding: 0rpx 20rpx;
		margin:20rpx 20rpx;
		-webkit-box-shadow: 0 0 20rpx 4rpx rgba(43,86,112,.1) ;
		box-shadow: 0 0 20rpx 4rpx rgba(43,86,112,.1) ;
		.title{
			max-width: 10px;
			overflow: hidden;
			font-size: 35rpx;
			font-weight: 700;
			white-space: nowrap;
		}
		.info{
			.tag{
				margin-right: 20rpx;
			}
			.value{
				background: #09bb07;
				padding: 0 10rpx;
				border-radius: .4rem ;
			}
		}
	}
	.device-info{
		flex-grow: 1;
		align-self: flex-start;
		// padding-top: 10rpx;
	}
	.artpic{
		flex-grow: 0;
		padding: $picpad+10 $picpad $picpad $picpad;
		align-self: center;
		image{
			margin-right: 20rpx;
			width: 260rpx;
			height: 180rpx;
		}
	}
	.oBorder{
	    border: none;
	    border-radius: 1.5rem ;
	}
</style>
