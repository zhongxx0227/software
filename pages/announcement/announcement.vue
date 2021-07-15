<template>
	<view class="" slot="head">
		<view style="height: 80rpx;"></view>
		<view style="align-items: center;">
			<u-row gutter="16">
				<u-col span="2">
				</u-col>
				<!-- 之后需要对字体居中、大小进行修改-->
				<u-col span="8">
					<view style="text-align: center; font-size: 80rpx;">
						发布公告
					</view>
				</u-col>
				<u-col span="2">
					<u-button @click="goto('./history')" style="font-size: 75rpx; height: 150rpx;" type="success">查看公告</u-button>
				</u-col>
			</u-row>
		</view>

		<view style="height: 300rpx;"></view>
		<view style="display: flex; align-items: center;justify-content: center; font-size: 100rpx;">
			<!--content与输入双向绑定-->
			<u-input v-model="content" :type="type" :border="border" :auto-height="autoHeight" placeholder="请输入公告内容"
				placeholder-style="font-size:100rpx" style="width: 1500rpx;height: 900rpx; font-size: 100rpx; display: flex; justify-content: center;
				align-items: center;" maxlength="150"/>
		</view>
		<view style="height: 300rpx;"></view>
		<!--不用view-->
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="3">
				<view>
					<u-button type="warning" @click="cancel" style="font-size: 100rpx; height: 150rpx;">取消</u-button>
				</view>
			</u-col>
			<u-col span="2"></u-col>
			<u-col span="3">
				<view>
					<u-button type="primary" @click="confirm" style="font-size: 100rpx; height: 150rpx;">确认</u-button>
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: "",
				content: "",
				type: "textarea",
				border: true,
				height: "500rpx",
				autoHeight: false
			}
		},
		methods: {
			cancel() {
				location.reload();
			},
			confirm() {
				uni.request({
					url: "http://192.168.1.104:8000/admin/put",
					method: "POST",
					data: {
						id: uni.getStorageSync('id'),
						content: this.content
					}
				})
				console.log(this.content)
				success: (res) => {
					console.log(res.data.msg)
					this.$u.toast("发布成功");
				}
				location.reload();
			},
			goto(url) {
				console.log("跳转")
				uni.navigateTo({
					url: url
				})
			}
		}
	}
</script>

<style>
</style>
