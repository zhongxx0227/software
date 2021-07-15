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
					增加用户
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
		<view style="height: 300rpx;"></view>
		<!--头像-->
		<view class="head_content">
			<u-upload :action="action" :auto-upload="true" @on-remove="remove" @on-success="success" max-count="1">
			</u-upload>
		</view>


		<view style="height: 200rpx;"></view>
		<u-card padding="10">
			<view class="" slot="body">
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
						<u-input v-model="创建用户名" type="text" height="200rpx" placeholder="请输入用户名"></u-input>
					</u-col>
				</u-row>
				<view style="height: 100rpx;"></view>
				<u-row gutter="16">
					<u-col span="2">
						<view class="demo-layout bg-purple">
							<view style="text-align: center; font-size: 60rpx;">
								密码
							</view>
						</view>
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="10">
						<u-input v-model="密码" type="password" height="200rpx"></u-input>
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
						<u-input v-model="确认密码" type="password" height="200rpx"></u-input>
					</u-col>
				</u-row>
			</view>
		</u-card>
		<view style="height: 100rpx;"></view>
		<view class="">
			<view v-if="BOSS">
				<u-dropdown title-size="50rpx">
					<u-dropdown-item v-model="身份" title="身份" :options="optionBoss"></u-dropdown-item>
				</u-dropdown>
			</view>
			<view v-else>
				<u-dropdown title-size="50rpx">
					<u-dropdown-item v-model="身份" title="身份" :options="optionNorm"></u-dropdown-item>
				</u-dropdown>
			</view>
		</view>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="4">
				<view>
					<u-button @click="cancel">取消</u-button>
				</view>
			</u-col>
			<u-col span="4">
				<view>
					<u-button @click="confirm">确认</u-button>
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
				formdata: {},
				管理员id: 1,
				创建用户名: "",
				密码: "",
				确认密码: "",
				身份: "",
				msg: "",
				BOSS: false,
				optionNorm: [{
						label: "点餐员",
						value: "点餐员"
					},
					{
						label: "后厨",
						value: "后厨"
					}
				],
				optionBoss: [{
						label: "点餐员",
						value: "点餐员"
					},
					{
						label: "后厨",
						value: "后厨"
					},
					{
						label: "管理员",
						value: "管理员"
					}
				],
				action: 'http://192.168.1.104:8000/admin/upload_pic'
			}
		},
		onLoad() {
			if (uni.getStorageSync('name') == "BOSS") {
				this.BOSS = true
			} else {
				this.BOSS = false
			}
		},
		methods: {
			cancel() {
				console.log("取消成功")
				uni.switchTab({
					url: './user'
				})
			},
			confirm() {
				if (this.密码 != this.确认密码) {
					this.$u.toast("请确认两次密码一致!");
					location.reload();
				} else {
					uni.request({
						url: "http://192.168.1.104:8000/admin/create_user",
						method: "POST",
						data: {
							管理员id: uni.getStorageSync('id'),
							创建用户名: this.创建用户名,
							密码: md5Libs.md5(this.密码),
							确认密码: md5Libs.md5(this.确认密码),
							身份: this.身份,
							图片: this.msg
						},
						success: (res) => {
							// console.log(this.$data)
							uni.switchTab({
								url: "./user"
							})
						}
					})
				}
			},
			remove() {
				console.log("图片移出")
			},
			success(data, index, lists, name) {
				console.log(data)
				this.msg = data.msg
			},
			back() {
				uni.switchTab({
					url: "./user"
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

	.wrap {
		padding: 24rpx;
	}

	.slot-btn {
		width: 341rpx;
		height: 140rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		background: rgb(244, 245, 246);
		border-radius: 10rpx;
	}

	.slot-btn__hover {
		background-color: rgb(235, 236, 238);
	}

	.pre-box {
		display: flex;
		align-items: center;
		justify-content: space-between;
		flex-wrap: wrap;
	}

	.pre-item {
		flex: 0 0 48.5%;
		border-radius: 10rpx;
		height: 140rpx;
		overflow: hidden;
		position: relative;
		margin-bottom: 20rpx;
	}

	.pre-item-image {
		width: 100%;
		height: 140rpx;
	}

	.head_content {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 60rpx;
	}
</style>
