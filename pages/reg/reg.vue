<template>
	<view class="container">
		<view style="margin: 0 50upx;z-index: 1000;">
			<view class="log-input">
				<text>手机号码</text>
				<input type="text" v-model="account" name="account" maxlength="11" placeholder="请输入手机号码">
			</view>
			
			<view class="code">
				<input type="number" v-model="verifycode" name="verifycode" maxlength="6" placeholder="请输入六位验证码">
				<button v-if="countdown>61" class="check" @tap="verification">获取验证码</button>
				<button v-if="countdown==61" class="check" @tap="verification">重新获取</button>
				<button v-if="countdown<61" class="check" disabled="true">{{countdown}}秒</button>
			</view>
			
			<view class="log-input">
				<text>用户名</text>
				<input type="text" v-model="nickname" name="nickname" maxlength="20" placeholder="请输入您的用户名">
			</view>
		
			<view class="log-input">
				<text>登录密码</text>
				<input type="text" v-model="pwd" name="pwd" password="true" maxlength="20" placeholder="登录密码至少六位数字加字母">
			</view>
		
			<view class="log-input">
				<text>确认密码</text>
				<input type="text" v-model="repwd" name="repwd" password="true" maxlength="20" placeholder="请再次确认您的登录密码">
			</view>
		
			<view class="log-input">
				<text>支付密码</text>
				<input type="text" v-model="pwd2" name="pwd2" password="true" maxlength="20" placeholder="必须填六位的支付密码">
			</view>
		
			<view class="log-input">
				<text>确认密码</text>
				<input type="text" v-model="repwd2" name="repwd2" password="true" maxlength="20" placeholder="请再次确认您的支付密码">
			</view>
			
			<view class="agreement">
				点击注册即为同意<navigator class="agr_text" url="/pages/reg/agree">《用户注册协定》</navigator>
			</view>
			
			<block v-if="status == 1">
				<button class="btn-area" @tap="submit">注册</button>
			</block>
			<block v-else>
				<button class="btn-area">注册</button>
			</block>
		</view>
	</view>
</template>

<script>
	//import service from '../../service.js';
	//import uniIcon from "@/components/uni-icon.vue";
	export default {
		components: {
			//uniIcon
		},
		data() {
			return {
				nickname: '',
				account: '',
				pwd: '',
				repwd: '',
				pwd2: '',
				repwd2: '',
				verifycode: '',
				countdown:62,
				status:1
			}
		},

		onShow: function() {
			
		},
		methods: {
			back(){
				uni.reLaunch({
					url: '/pages/login/login',
				});
			},
			verification(){
				self = this;
				if(self.account.length != 11){
					uni.showToast({
						icon: 'none',
						title: '请输入11位的手机号码'
					});
					return;
				}
				
				//手機號碼正則驗證
				if(!(/^1[3|4|5|7|8|9][0-9]\d{8,11}$/.test(self.account))){
					uni.showToast({
						icon: 'none',
						title: '手机号码格式不正确'
					});
					return;
				}
				//發送短信驗證碼
				uni.request({
					url: service.api.reg.code,
					method: 'GET',
					data: {
						'mobile': self.account,
						'temp': 'sms_reg',
					},
					success: res => {
						var res = res.data;
						if (res.code == 1) {
							uni.showToast({
								icon: 'none',
								title: res.msg
							});
							this.codetime(this,60);
						} else {
							uni.showToast({
								icon: 'none',
								title: res.msg
							});
							return;
						}
					}
				});
			},
			//手機驗證碼獲取倒計時
			codetime: function(obj,num) {
				if(num<10){
					obj.countdown = "0"+num;
				}else{
					obj.countdown = num;
				}
				num--;
				if(num < 0){
					obj.countdown = 61;
				}else{
					setTimeout(function(){
						obj.codetime(obj,num);
					}, 1000);
				}
			},
			submit() {
				self = this;
				
				if(self.account.length != 11){
					uni.showToast({
						icon: 'none',
						title: '请输入11位的手机号码'
					});
					return;
				}
				
				//手機號碼正則驗證
				if(!(/^1[3|4|5|7|8|9][0-9]\d{8,11}$/.test(self.account))){
					uni.showToast({
						icon: 'none',
						title: '手机号码格式不正确'
					});
					return;
				}
				if(self.verifycode.length != 6){
					uni.showToast({
						icon: 'none',
						title: '请输入六位验证码'
					});
					return;
				}
				if(self.nickname.length < 2){
					uni.showToast({
						icon: 'none',
						title: '请输入至少两位字符的用户名'
					});
					return;
				}
				if(self.pwd.length < 6){
					uni.showToast({
						icon: 'none',
						title: '请输入至少六位数字加字母的登录密码'
					});
					return;
				}
				if(self.repwd != self.pwd){
					uni.showToast({
						icon: 'none',
						title: '确认登录密码与设定的登录密码不一致'
					});
					return;
				}
				if(self.pwd2.length != 6){
					uni.showToast({
						icon: 'none',
						title: '请输入六位的支付密码'
					});
					return;
				}
				if(self.repwd2 != self.pwd2){
					uni.showToast({
						icon: 'none',
						title: '确认支付密码与设定的支付密码不一致'
					});
					return;
				}
				self.status = 0;
				uni.request({
					url: service.api.reg.register,
					method: 'POST',
					data: {
						account:self.account,
						verifycode:self.verifycode,
						nickname:self.nickname,
						pwd:self.pwd,
						repwd:self.repwd,
						pwd2:self.pwd2,
						repwd2:self.repwd2
					},
					success: res => {
						var res = res.data;
						if (res.code == 1) {
							uni.showModal({
								title: '注册成功',
								content: res.msg,
								showCancel: false,
								success: function(res) {
									uni.reLaunch({
										url: '/pages/login/login'
									});
								}
							});
						}else{
							uni.showModal({
								title: '注册失败',
								content: res.msg,
								showCancel: false,
								success: function(res) {
									self.status = 1;
								}
							});
						}
					},
					complete: () => {
						uni.hideLoading()
					}
				});
			}
		}
	}
</script>

<style lang='scss'>
	page{
		background: #F1F1F1;
	}
	.container{
		position:relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	.log-lable {
		width: 100%;
		font-size: 48upx;
		font-weight: bold;
		margin-top: 120upx;
		text-align: center;
		color: rgba(0, 0, 0, 1);
	}
	.code input{
		padding-left: 30upx;
	}
	uni-page-head .uni-page-head-hd{
		display: block;
	}
	.log-input {
		width: auto;
		height: 90upx;
		background: white;
		margin-top: 42upx;
		line-height: 90upx;
		padding-left: 40upx;
		border-radius: 45upx;
	}

	.log-input text {
		width: 30%;
		float: left;
		font-size: 24upx;
		text-align: justify;
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
	
	.agreement{
		font-size: 28upx;
		text-align: right;
		margin-top: 40upx;
	}
	
	.agr_text{
		float: right;
		color: #5D8AFC;
		margin-right: 120upx;
	}

	.oauth-row {
		top: 0;
		left: 0;
		width: 100%;
		display: flex;
		position: absolute;
		flex-direction: row;
		justify-content: center;
	}

	.oauth-image {
		width: 100upx;
		height: 100upx;
		margin: 0 40upx;
		border-radius: 100upx;
		background-color: #ffffff;
		border: 2upx solid #dddddd;
	}

	.oauth-image image {
		width: 60upx;
		height: 60upx;
		margin: 20upx;
	}
</style>
