<template>
	<view>
		<view v-if="current === 0">
			<info></info>
		</view>
		<view v-if="current === 1">
			<control></control>
		</view>
		<view v-if="current === 2">
			<history></history>
		</view>
		<tab-bar></tab-bar>
	</view>
</template>

<script>
	import tabBar from '../../components/tabBar/tabBar.vue';
	import info from './info.vue'
	import control from './control.vue'
	import history from './history.vue'
	export default {
		components:{
			info,
			control,
			history,
			tabBar
		},
		data() {
			return {
				current: 0
			}
		},
		onLoad:function(option){
			var that = this;
			uni.$on("updateindex", function(index){
				that.current = index;
			});
			if (undefined != option.device){
				let devlist = uni.getStorageSync("devlist");
				for (let dev in devlist) {
					if (devlist[dev].devid == option.device){
						uni.setStorageSync("device", devlist[dev]);
						break;
					}
				}
			}
		},
		methods: {
			
		},
		computed:{
			devid(){
				return uni.getStorageSync("device").devid
			}
		}
	}
</script>

<style>
	page{
		background-color: #fcfcfc;
	}
</style>
