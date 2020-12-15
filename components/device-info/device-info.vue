<template>
	<navigator animation-type="zoom-out" animation-duration="300" :url="url">
		<view class="device-card oBorder">
			<view class="artpic">
				<image src="../../static/pic.jpg" class="oBorder"></image>
			</view>
			<view class="device-info">
				<text class="title">{{ this.devdata.name }}</text>
				<hr></hr>
				<view class="info">
					<text class="tag">温度</text>
					<text class="value">{{ this.termperature }}℃</text>
				</view>
				<view class="info">
					<text class="tag">湿度</text>
					<text class="value">{{ this.humidity }}%</text>
				</view>
				<view class="info">
					<text class="tag">光强</text>
					<text class="value">{{ this.intensity }}Lux</text>
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
			console.log("外你");
			console.log(this.devdata.devid);
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
				complete: () => {}
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
		padding: 0rpx 20rpx;
		margin:20rpx 0rpx;
		.title{
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
	    -webkit-box-shadow: 0 0 30rpx 0 rgba(53, 153, 17, 0.1) ;
	    box-shadow: 0 0 60rpx 0 rgba(53, 153, 17, 0.1);
	}
</style>
