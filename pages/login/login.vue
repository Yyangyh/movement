<template>
	<view class="content">
		<view class="login">
			<view class="log-lable">
				<image :src="logo" mode="aspectFit" style="height: 126upx;"></image>
			</view>
			
			<view class='log'>
				<view class="log-input">
					<text>账&nbsp;&nbsp;&nbsp;号</text>
					<input type="text" v-model="username" maxlength="20" placeholder="请输入您的账号">
				</view>

				<view class="log-input">
					<text>密&nbsp;&nbsp;&nbsp;码</text>
					<input type="text" v-model="password" password="true" maxlength="20" placeholder="请输入您的密码">
				</view>

				<block v-if="status == 1">
					<button class="btn-area" @tap="bindLogin">提交</button>
				</block>
				<block v-else>
					<button class="btn-area">提交</button>
				</block>
				
				<view class='log-bottom'>
					<navigator url="../login/forget" class="bottom-left">忘记密码</navigator>
					<navigator url="../reg/reg" class="bottom-right">我要注册</navigator>
				</view>
			</view>
		
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				height:'',
				username: '',
				password: '',
				status:1,
				logo: uni.getStorageSync('logo')
			};
		},
		methods: {
			bindLogin() {
				self = this;
				if (self.username.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '账号最短为1位字符'
					});
					return;
				}
				if (self.password.length < 6) {
					uni.showToast({
						icon: 'none',
						title: '密码最短为6个字符'
					});
					return;
				}
				self.status = 0;
				uni.request({
					url: service.api.login.login,
					method: 'POST',
					data: {
						'account': self.username,
						'password': self.password
					},
					success: res => {
						var res = res.data;
						if (res.code == 1) {
							uni.reLaunch({
								url: '/pages/index/index',
							});
						} else {
							self.status = 1;
							self.password = '';
							service.removeToken();
							uni.showToast({
								icon: 'none',
								title: res.msg
							});
							return;
						}
					},
					error: res => {
						uni.showToast({
							icon: 'none',
							title: "请求站点信息错误"
						});
						return;
					}
				});
			},
		},
		onShow:function() {
			uni.request({
				url: service.api.login.logo,
				method: 'GET',
				success: res => {
					var res = res.data;
					if (res.code == 1) {
						this.logo = res.data.site_logo;
					}
				}
			})
		}
	}
</script>

<style>
	page{
		background: #F7F7F7;
	}
	.back_img{
		width: 100%;
		height: 100%;
		position: fixed;
	}
	.login{
		z-index: 998;
		padding: 90% 0upx 0upx;
	}
	.log{
		padding: 100upx 70upx 0upx;
	}
	.log-lable {
		font-size: 48upx;
		font-weight: bold;
		text-align: center;
		margin-top: -400upx;
		color: rgba(0, 0, 0, 1);
	}

	.log-prompt {
		width: 100%;
		height: 24upx;
		font-size: 24upx;
		font-weight: 500;
		margin-top: 80upx;
		line-height: 46upx;
		color: rgba(153, 153, 153, 1);
	}
	
	.log-input {
		height: 90upx;
		line-height: 90upx;
		padding-left: 40upx;
		background: #FFFFFF;
		border-radius: 45upx;
		margin-bottom: 40upx;
	}

	.log-input text {
		width: 25%;
	}

	input {
		width: 400upx;
	}

	.log-remark {
		width: 100%;
		height: 18upx;
		font-size: 19upx;
		font-weight: 500;
		margin-top: 30upx;
		line-height: 46upx;
		color: rgba(101, 101, 101, 1);
	}

	.btn-area {
		margin: 60upx 0upx;
	}
	.log-bottom navigator{
		color:#ffa100;
		font-size:30upx;
		font-weight:400;
	}
	.bottom-left{
		float: left;
	}
	.bottom-right{
		float: right;
	}
</style>
