<template>
	<view class="page">
		<view class="back">
			<uni-icon size="28" type="arrowleft" @click="back()"></uni-icon>
		</view>
		
		<view style="margin: 40upx 38upx;">
			<div v-html="terms"></div>
		</view>
	</view>
</template>

<script>
	//import service from '../../service.js';
	//import uniIcon from "@/components/uni-icon.vue";
	export default {
		data() {
			return {
				terms: ''
			};
		},
		components: {
			//uniIcon
		},
		onshow: function() {
			uni.request({
				url: service.api.reg.agree,
				method: 'GET',
				success: res => {
					var res = res.data;
					if (res.code == 1) {
						this.terms = res.data.registration_agreement;
					}
				}
			})
		},methods:{
			back(){
				uni.reLaunch({
					url: '/pages/reg/reg',
				});
			}
		}
	}
</script>

<style>
	page{
		background: #F7F7F7;
	}
</style>
