<template>
	<navigator animation-type="zoom-out" animation-duration="300" url="../../pages/sub/devctrl?device=35cff258-676a-483f-bea0-01089d72c34t">
		<view class="device-card oBorder">
			<view class="artpic">
				<image src="../../static/pic.jpg" class="oBorder"></image>
			</view>
			<view class="device-info">
				<text class="title">不知道是啥</text>
				<hr></hr>
				<view class="info">
					<text class="tag">温度</text>
					<text class="value">1℃</text>
				</view>
				<view class="info">
					<text class="tag">光强</text>
					<text class="value">152lux</text>
				</view>
				<view class="info">
					<text class="tag">湿度</text>
					<text class="value">50%</text>
				</view>
			</view>
		</view>
	</navigator>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods: {
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
