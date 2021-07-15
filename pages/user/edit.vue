<template>

	<view class="">
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
				<u-icon name="arrow-left" size="100rpx" @click="back"></u-icon>
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="8">
				<view style="text-align: center; font-size: 80rpx;">
					修改用户
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
		<view style="height: 100rpx;"></view>
		<!--需要改成居中，界面布局-->
		<!-- <u-image width="80%" height="500rpx" style="align-content: center;" :src="src"></u-image> -->
		</u-row>
		<view>
			<view v-if="seen">
				<view style="height: 100rpx;"></view>
				<view class="orgimg">
					<u-image width="80%" height="1000rpx" :src="图片" mode="aspectFit" @click="changeImg"></u-image>
				</view>
			</view>
			<view v-else>
				<view style="height: 100rpx;"></view>
				<view class="head_content">
					<u-upload :action="action" :auto-upload="true" @on-success="success" max-count="1">
					</u-upload>
				</view>
			</view>
		</view>
		<u-card padding="10">
			<view class="" slot="body">
				<view style="height: 100rpx;"></view>
				<u-row gutter="16">
					<u-col span="2">
						<view class="demo-layout bg-purple">
							<view style="text-align: center; font-size: 60rpx;">
								用户名
							</view>
						</view>
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="10">
						<u-input v-model="用户名" height="200rpx" placeholder="请输入用户名"></u-input>
					</u-col>
				</u-row>
				<view style="height: 100rpx;"></view>
				<u-row gutter="16">
					<u-col span="2">
						<view class="demo-layout bg-purple">
							<view style="text-align: center; font-size: 60rpx;">
								新密码
							</view>
						</view>
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="10">
						<u-input v-model="newPwd" type="password" height="200rpx" placeholder="请输入密码"></u-input>
					</u-col>
				</u-row>
				<view style="height: 100rpx;"></view>
				<u-row gutter="16">
					<u-col span="2">
						<view class="demo-layout bg-purple">
							<view style="text-align: center; font-size: 60rpx;">
								确认密码
							</view>
						</view>
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="10">
						<u-input v-model="conNewPwd" type="password" height="200rpx" placeholder="请确认密码"></u-input>
					</u-col>
				</u-row>
			</view>
		</u-card>
		<view style="height: 100rpx;"></view>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="4">
				<view>
					<u-button @click="cancel" type="warning" style="font-size: 100rpx; height: 150rpx; width: 500rpx;">
						取消</u-button>
				</view>
			</u-col>
			<u-col span="4">
				<view>
					<u-button @click="confirm" type="primary" style="font-size: 100rpx; height: 150rpx; width: 500rpx;">
						确认</u-button>
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
	</view>

</template>

<script>
	import md5Libs from "uview-ui/libs/function/md5";
	export default {
		data() {
			return {
				员工id: 0,
				用户名: "",
				newPwd: "",
				conNewPwd: "",
				新密码: "",
				确认密码: "",
				图片: "",
				身份: "",
				seen: true,
				action: 'http://192.168.1.104:8000/admin/upload_pic'
			}
		},
		onLoad: function(options) {
			let list = JSON.parse(options.id)
			console.log(list)
			this.图片 = list.图片
			this.用户名 = list.用户名
			this.新密码 = list.密码
			this.身份 = list.身份
			this.员工id = list.员工id
			console.log(this.$data)
		},
		methods: {
			goto() {
				console.log("跳转")
				uni.switchTab({
					url: "./user"
				})
			},
			cancel() {
				uni.switchTab({
					url: "./user"
				})
			},
			confirm() {
				console.log(this.newPwd)
				console.log(this.conNewPwd)
				if (this.newPwd && this.conNewPwd) {
					if (this.newPwd != this.conNewPwd) {
						uni.$u.toast("请确认密码!")
						location.reload()
					} else {
						this.新密码 = md5Libs.md5(this.newPwd)
						this.确认密码 = md5Libs.md5(this.conNewPwd)
					}
				}
				console.log(this.$data)
				uni.request({
					url: 'http://192.168.1.104:8000/admin/modify_user',
					method: "POST",
					data: {
						管理员id: uni.getStorageSync('id'),
						员工id: this.员工id,
						创建用户名: this.用户名,
						密码: this.新密码,
						图片: this.图片
					},
					success: (res) => {
						
						if (res.data.msg == "True") {
							console.log("成功")
							uni.switchTab({
								url: "./user"
							})
						} else {
							// location.reload()
							console.log("失败")
						}

					}
				})

			},
			changeImg() {
				this.seen = false
			},
			success(data, index, lists, name) {
				console.log(data.msg)
				this.图片 = data.msg
			},
			back(){
				uni.switchTab({
					url:"./user"
				})
			}
		}

	}
</script>

<style>
	.profile {
		align-content: center;
	}

	.demo-layout {
		height: 80rpx;
		border-radius: 8rpx;
	}

	.bg-purple {
		background: #d3dce6;
	}

	.bg-grey {
		background: #DCDCDC;
	}

	.orgimg {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.head_content {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 60rpx;
	}
</style>
