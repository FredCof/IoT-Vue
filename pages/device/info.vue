<template>
	<view class="control-page">
		<view class="header">
			<image src="../../static/pico.jpg"></image>
		</view>
		<view class="des-card">
			<view class="setline goodinfo">
				<text>藏品名字：马踏飞燕</text>
				<text>藏品编号：0002023</text>
			</view>
			<view class="setline">
				<text class="des-text">
					温度
				</text>
				<image src="../../static/Temper.png" mode="widthFix"></image>
				<text class="width-set">{{ info.t }}℃</text>
				<uni-tag v-if="info.t >= setting.tMax" text="偏高" type="error" :circle="true"></uni-tag>
				<uni-tag v-else-if="info.t <= setting.tMin" text="偏低" type="warning" :circle="true"></uni-tag>
				<uni-tag v-else text="正常" type="success" :circle="true"></uni-tag>
			</view>
			<view class="setline">
				<text class="des-text">
					湿度
				</text>
				<image src="../../static/Hud.png" mode="widthFix"></image>
				<text class="width-set">{{ info.h }}%</text>
				<uni-tag v-if="info.h >= setting.hMax" text="偏高" type="error" :circle="true"></uni-tag>
				<uni-tag v-else-if="info.h <= setting.hMin" text="偏低" type="warning" :circle="true"></uni-tag>
				<uni-tag v-else text="正常" type="success" :circle="true"></uni-tag>
			</view>
			<view class="setline">
				<text class="des-text">光强</text>
				<image src="../../static/Sun.png" mode="widthFix"></image>
				<text class="width-set">{{ info.l }}Lux</text>
				<uni-tag v-if="info.l >= setting.iMax" text="偏高" type="error" :circle="true"></uni-tag>
				<uni-tag v-else-if="info.l <= setting.iMin" text="偏低" type="warning" :circle="true"></uni-tag>
				<uni-tag v-else text="正常" type="success" :circle="true"></uni-tag>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				maxTemp: 0,
				minTemp: 0,
				maxIns: 0,
				minIns: 0,
				maxHud: 0,
				minHud: 0,
			}
		},
		methods: {
			
		},
		computed:{
			devid(){
				return uni.getStorageSync("device").devid;
			},
			info(){
				return uni.getStorageSync("device").info;
			},
			setting(){
				return uni.getStorageSync("device").setting;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.goodinfo{
		font-size: 25rpx !important;
	}
	.control-page{
		display: flex;
		flex-direction: column;
		justify-content:center;
	}
	.width-set{
		width: 200rpx;
		text-align: center;
	}
	.header {
		box-shadow:0rpx 0rpx 60rpx 10rpx rgba(0,0,0,0.1);
		overflow: hidden;
		width: 400rpx;
		height: 400rpx;
		border-radius:50%;
		margin: 100rpx auto 80rpx;
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
		margin: 30rpx 20rpx;
		font-size: 35rpx;
		justify-content: space-between;
		align-items: center;
		.des-text{
			align-self: center;
		}
		.des-blod{
			font-size: 40rpx;
			font-weight: 900;
		}
		image{
			width: 64rpx;
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
		image{
			margin: 10rpx;
			height: 60rpx;
		}
		&::after{
			border: none;
		}
	}
</style>
