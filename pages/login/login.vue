<template>
	<view class="conten">

		<u-mask :show="show_mask" :custom-style="{background: 'rgba(235, 235, 235, 0.5)'}">
			<view class="maskDis">
				<u-loading mode="flower" size="70" :show="show_loading"></u-loading>
				<text class="masktext" style="color: #EAF6F9;padding-left: 40rpx;">登录中</text>
			</view>
			<u-modal v-model="show_model" :content="model_content" :async-close="true" @confirm="model_confirm">
			</u-modal>
		</u-mask>

		<view class="content">
			<!-- 图表显示层 -->
			<view class="avatorWrapper">
				<view class="avator">
					<image class="img" src="../../static/logo.png" mode="widthFix"></image>
				</view>
			</view>

			<view class="form">
				<u-form :model="form">
					<u-form-item label=" :" prop="account" label-width="95px" class="form-item" left-icon="account">
						<u-input class="inputWrapper" placeholder="请输入用户名" v-model="form.account" />
					</u-form-item>
					<u-form-item label=":" prop="pwd" label-width="95px" class="form-item" left-icon="lock">
						<u-input class="inputWrapper" placeholder="请输入密码" v-model="form.pwd" type="password" />
					</u-form-item>
				</u-form>

			</view>

			<view class="bottom">
				<view class="message">
					<xl-slider-verify v-if="reflesh" @success="verifySuccess" :reset="false"></xl-slider-verify>
				</view>

				<view class="loginBtn">
					<button type="primary" class="loginbtn" :disabled="button_albe" plain
						@click="forsubmit()">登录</button>
				</view>

				<view class="forgotBtn">
					<text class="comtext">找回密码</text>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	import md5Libs from "uview-ui/libs/function/md5";
	import xlSliderVerify from '@/pages/login/xl-slider-verify.vue';
	export default {
		components: {
			xlSliderVerify
		},
		data() {
			return {
				show_mask: false,
				show_loading: false,
				show_model: false,
				button_albe: true,
				reflesh: true,
				model_content: "", //model
				form: {
					account: "",
					pwd: ""
				},
				ruls: {
					account: [],
					pwd: []
				},
				// baseURL:"https://www.fastmock.site/mock/6e815471dd0658044529034105eab31e/chef",
				baseURL: "http://192.168.1.104:8000/login"
			}
		},
		onLoad() {
			if (this.form.account && this.form.pwd) {
				location.reload()
			}
		},
		methods: {
			forsubmit(e) {
				console.log("发生提交")
				let _this = this
				//弹出遮罩层
				_this.show_mask = true
				_this.show_loading = true
				//发送请求
				uni.request({
					url: _this.baseURL,
					method: 'POST',
					data: {
						'name': _this.form.account,
						'password': md5Libs.md5(_this.form.pwd),
						'code1': "2345",
						'code2': "2345"
					},
					dataType: 'json',
					//数据返回处理
					success: (res) => {
						//对服务器返回消息进行判断
						if (res.data.msg == "True") { //登录成功
							_this.show_loading = false
							_this.show_mask = false
							uni.setStorageSync('id', res.data.data.id);
							uni.setStorageSync('name', res.data.data.name);
							uni.setStorageSync('job', res.data.data.job);
							uni.setStorageSync('src',res.data.data.pic);
							uni.switchTab({
								url: "../food/food"
							})
						} else if (res.data.msg == "False") {
							_this.model_content = "登录失败,请检查用户或密码"
							_this.reflesh = false
							_this.show_model = true
							_this.reflesh = true
						}

					}
				});
			},
			formreset() {

			},
			model_confirm() {
				this.show_model = false
				this.show_mask = false
			},
			verifySuccess() {
				this.button_albe = false
				console.log("验证成功")
			}
		}
	}
</script>

<style>
	.content {
		background: #ffffff;
	}

	.avatorWrapper {
		height: 25vh;
		width: 100vw;
		display: flex;
		justify-content: center; 
		align-items: flex-end;
	}

	.avator {
		width: 200upx;
		height: 200upx;
		overflow: hidden;
	}

	.avator .img {
		width: 100%
	}

	.form {
		padding: 0 212px;
		margin-top: 80px;
		align-items: center;
		height: 15vh;
	}

	.inputWrapper {
		width: 439.45rpx;
		border-radius: 20px;
		text-align: center;
	}

	.input {
		width: 100%;
		height: 100%;
		text-align: center;
		font-size: 25px;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: #e6f4fc;
		border-radius: 20px;
	}

	.loginBtn {
		height: 150upx;
		background: white;
		border-radius: 20px;
		box-sizing: border-box;
	}

	.loginbtn {
		height: 150upx;
		width: 600px;
		font-size: 60rpx;
		text-align: center;
		border-radius: 20px;
		margin-top: 40px;
	}

	.comtext {
		font-size: 25px;
	}

	.forgotBtn {
		text-align: center;
		color: #000000;
		font-size: 15px;
		margin-top: 20px;
	}

	.maskDis {
		display: flex;
		width: 600px;
		height: 70px;
		align-items: center;
		text-align: center;
		padding-left: 420rpx;
		margin-top: 200px;
		margin-left: 212px;
		background-color: #626262;
		text-align: center;

	}

	.masktext {
		font-size: 30px;
		text-align: center;
		padding-top: 5px;
		margin-bottom: 7px;
	}

	.message {
		height: 150rpx;
		width: 750rpx;
		margin-top: 21.97rpx;
		padding: 0px 212px;
	}

	.bottom {
		align-items: center;
	}

	.form-item {
		height: 150rpx;
		font-size: 25px;
		padding-top: 20px;
		background-color: #f0f0f0;
		border-radius: 10px;
		margin-bottom: 10px;
	}
</style>
