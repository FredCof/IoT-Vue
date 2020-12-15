<template>
	<view class="zdTabBar">
		<view class="ul">
			<view :class="['li',cur == index?'cur' :'']" v-for="(item,index) in taBbarList" :key="index" @tap="navigatorTo(item, index)">
				<view :class="['tabicon' ,cur == index?'ic' :'img']">
					<image :src="cur == index ? item.selectIcon : item.icon" mode="widthFix"></image>
				</view>
				<view class="p">{{item.name}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			current: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				cur: 0,
				taBbarList: [{
						type: 0,
						icon: '/static/MenuNo.png',
						selectIcon: '/static/Menu.png',
						name: '信息',
						title: '设备信息',
						url: '/pages/device/info'
					},
					{
						type: 1,
						icon: '/static/OptionNo.png',
						selectIcon: '/static/Option.png',
						name: '控制',
						title: '设备控制',
						url: '/pages/device/control'
					},
					{
						type: 0,
						icon: '/static/BoxNo.png',
						selectIcon: '/static/Box.png',
						name: '历史记录',
						title: '历史信息',
						url: '/pages/device/history'
					}
				]
			}
		},
		methods: {
			navigatorTo(e, i) {
				this.cur = i;
				uni.setNavigationBarTitle({
				    title: e.title
				});
				uni.$emit('updateindex', i);
			}
		}
	}
</script>
<style lang="less">
	@mainc: #00B7B8; //主色
	@textc: #464646; //字体颜色
	@textca: #909090; //字体颜色1
	@borderc: #eeeeee; //边框颜色
	@redc: #47c05a; //红色字体
	image {
		display: block;
		width: 100%;
		height: 100%;
	}
	.df {
		display: flex;
	}
	page {
		font-size: 24upx;
		color: @textc;
		line-height: 35upx;
		padding-bottom: 60px;
		background: #f7f7f7;
		font-family:'微软雅黑', sans-serif, serif;
	}
	.bgja {
		background-image: linear-gradient(right, #00CEC1, #00B7B8);
		background-image: -webkit-linear-gradient(right, #00CEC1, #00B7B8);
	}
	.zdTabBar{
		position: fixed;
		left: 0;
		bottom: 0;
		right: 0;
		z-index: 20;
		background: #ffffff;
		.ul{
			.df;
			.li{
				flex: 1;
				padding-top: 5px;
				position: relative;
				.tabicon{
					transition: all ease-in-out .3s;
				}
				.img{
					width: 25px;
					height: 25px;
					overflow: hidden;
					margin: 0 auto;
				}
				.ic{
					width: 40px;
					height: 40px;
					border-radius: 50%;
					text-align: center;
					font-size: 20px;
					line-height: 40px;
					font-weight: bold;
					color: #ffffff;
					margin: -20px auto 5px;
					position: relative;
					background-color: #ffffff;
					-webkit-box-shadow: 0 0 20rpx 0 rgba(18, 124, 80, .5) ;
					box-shadow: 0 0 20rpx 0 rgba(18, 124, 80, .5) ;
					transform: scale(1.2);
					image{
						top: 50%;
						left: 50%;
						transform: translate(-50%, -50%);
						position: absolute;
						width: 60%;
					}
				}
				.p{
					margin-top: 2px;
					text-align: center;
					font-size: 12px;
					line-height: 18px;
					font-weight: 900;
					color: #666666;
				}
				&.cur{
					.p{
						color: @redc;
					}
				}
			}	
		}
	}
</style>