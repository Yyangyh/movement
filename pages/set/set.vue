<template>
	<view>
		<view class="content">
			<view class="list">
				<view class="row">
					<view class="title">头像</view>
					<view class="right">
						<view class="tis">
							<image src="/static/img/face.jpg" mode="widthFix"></image>
						</view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">昵称</view>
					<view class="right">
						<view class="tis">大黑哥</view>
					</view>
				</view>
				<view class="row">
					<view class="title">个性签名</view>
					<view class="right">
						<view class="tis">这人太懒了，什么都不写</view>
					</view>
				</view>
				<view class="row" @click="navTo('/pages/address/address')">
					<view class="title">收货地址</view>
					<view class="right">
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">账户安全</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
			</view>
			<view class="list">
				<view class="row">
					<view class="title">通知提醒</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">支付设置</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">通用</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
			</view>
			<view class="list">
				<view class="row">
					<view class="title">版本升级</view>
					<view class="right">
						<view class="tis">v1.0.0</view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">清除缓存</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">问题反馈</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
				<view class="row">
					<view class="title">关于商城</view>
					<view class="right">
						<view class="tis"></view>
						<uni-icon type="forward" size="20" class="icon"></uni-icon>
					</view>
				</view>
			</view>
		</view>
		<view class="list-cell log-out-btn" @click="toLogout">
			<text class="cell-tit">退出登录</text>
		</view>
	</view>
</template>

<script>
	import uniIcon from "@/components/uni-icon.vue";
	export default {
		components: {
			uniIcon
		},
		data() {
			return {
				
			};
		},
		onNavigationBarButtonTap(e) {
			uni.navigateTo({  
				url: '/pages/set/upload'
			})
		},
		methods: {
			showType(tbIndex){
				this.tabbarIndex = tbIndex;
				this.list = this.orderList[tbIndex];
			},
			navTo(url){
				uni.navigateTo({
					url: url,
				});
			},
			//退出登录
			toLogout(){
				uni.showModal({
				    content: '确定要退出登录么',
					confirmColor: '#FFAE00',
					cancelColor: '#D2D2D2',
				    success: (e)=>{
				    	if(e.confirm){
				    		setTimeout(()=>{
				    			uni.reLaunch({
				    				url: '/pages/login/login',
				    			});
				    		}, 1000)
				    	}
				    }
				});
			},
		}
	}
</script>

<style lang="scss">
	@import "@/store/style.scss";
page{
	background-color: #f3f3f3;	
}
	.list-cell{
		display:flex;
		align-items:baseline;
		padding: 20upx $page-row-spacing;
		line-height:60upx;
		position:relative;
		background: #ffa100;
		justify-content: center;
		&.log-out-btn{
			margin-top: 40upx;
			.cell-tit{
				color:#ffffff;
				text-align: center;
				margin-right: 0;
			}
		}
	}
.content{
	.list{
		width: 100%;
		padding-left: 4%;
		background-color: #fff;
		margin-bottom: 20upx;
		.row{
			widows: 100%;
			min-height: 90upx;
			display: flex;
			align-items: center;
			justify-content: space-between;
			border-bottom: solid 1upx #eee;
			&:last-child{
				border-bottom: none;
			}
			.title{
				font-size: 32upx;
				color: #333;
			}
			.right{
				display: flex;
				align-items: center;
				color: #999;
				.tis{
					font-size: 26upx;
					margin-right: 20upx;
					max-height: 120upx;
					image{
						width: 100upx;
						height: 100upx;
						border-radius: 100%;
						margin: 10upx 0;
					}
				}
				.icon{
					width: 40upx;
					color: #cecece;
				}
			}
			
		}
	}
}

</style>
