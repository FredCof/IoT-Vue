<template>
	<view>
		<view v-show="current === 0">
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
				uni.setStorageSync("devid", option.device)
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

<style>

</style>
