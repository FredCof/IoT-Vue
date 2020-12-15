<template>
	<view class="control-page">
		<view class="header">
			<image src="../../static/pic.jpg"></image>
		</view>
		<view class="des-card">
			<view class="setline">
				<text class="des-text">设定温度范围：</text>
				<view>
					<uni-number-box :max="50" :min="-10" :value="maxTemp" :control="false"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="50" :min="-10" :value="minTemp" :control="false"></uni-number-box>
				</view>
				<button class="sendbtn">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">设定湿度范围：</text>
				<view>
					<uni-number-box :max="50" :min="-10" :value="maxHud" :control="false"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="50" :min="-10" :value="minHud" :control="false"></uni-number-box>
				</view>
				<button class="sendbtn">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">设定光强范围：</text>
				<view>
					<uni-number-box :max="50" :min="-10" :value="maxIns" :control="false"></uni-number-box>
					<text class="des-text des-blod">~</text>
					<uni-number-box :max="50" :min="-10" :value="minIns" :control="false"></uni-number-box>
				</view>
				<button class="sendbtn">
					<image src="../../static/Cloud.png" mode="heightFix"></image>
				</button>
			</view>
			<view class="setline">
				<text class="des-text">下发权限</text>
				<button class="sendbtn">
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
				return uni.getStorageSync("devid")
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
	}
	.des-card{
		width: 90%;
		margin: 0 5%;
		border-radius: 1rem;
		padding: 30rpx 0;
		-webkit-box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
		box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
	}
	.setline{
		display: flex;
		margin: 10rpx 20rpx;
		font-size: 35rpx;
		.des-text{
			align-self: center;
		}
		.des-blod{
			font-size: 40rpx;
			font-weight: 900;
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
