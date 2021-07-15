<!-- 需要修改数据，修改为接口提供的，这样
-->

<template>
	<u-card padding="10">
		<!-- 标题栏-->
		<view class="" slot="head">
			<view style="align-items: center;">
				<u-row gutter="16">
					<u-col span="2">
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="8">
						<view style="text-align: center; font-size: 80rpx;">
							员工管理
						</view>
					</u-col>
					<u-col span="2">

						<u-button @click="goto('./add')" style="font-size: 75rpx; height: 150rpx;" type="success">增加用户
						</u-button>

					</u-col>
				</u-row>

			</view>
		</view>

		<view class="" slot="body">
			<view v-for="(item,index) in list">
				<u-row gutter="16">
					<u-col span="3">
						<!-- <u-image width="100%" height="300rpx" src="../../static/profile/头像.png"></u-image> -->
						<u-image width="100%" height="300rpx" :src="item.图片"></u-image>
					</u-col>
					<!-- 之后需要对字体居中、大小进行修改-->
					<u-col span="4">
						<view style="text-align: left; font-size: 100rpx; font-weight: 500rpx;">
							ID: {{item.员工id}}
						</view>
						<view style="text-align: left; font-size: 75rpx;">
							身份: {{item.身份}}
						</view>
						<view style="text-align: left; font-size: 50rpx;">
							用户名: {{item.用户名}}
						</view>
					</u-col>
					<u-col span="1">
					</u-col>
					<u-col span="2">
						<view>
							<!-- <button @click="gotoEdit('./edit', JSON.stringify(list[index]))" -->
							<button @click="judgeStatus('./edit',JSON.stringify(list[index]),index)"
								style="font-size: 70rpx; height: 130rpx; display: flex; align-items: center; justify-content: center;"
								type="primary">修改</button>
						</view>
					</u-col>
					<u-col span="2">
						<view>
							<button @click="userDelete(index)"
								style="font-size: 70rpx; height: 130rpx; display: flex; align-items: center; justify-content: center;"
								type="warn">删除</button>
						</view>
					</u-col>
				</u-row>



			</view>
		</view>

	</u-card>
</template>

<script>
	export default {
		data() {
			return {
				title: "员工管理",
				thumb: 'http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg',
				list: [],
				seen: false
			};
		},
		onShow() {
			this.requestData();
		},
		methods: {
			//跳转到增加用户页
			goto(url) {
				console.log("新增用户跳转")
				uni.navigateTo({
					url: url
				})
			},
			//数据传输
			requestData() {
				uni.request({
					url: "http://192.168.1.104:8000/admin/show_user",
					method: "POST",
					data: {
						id: uni.getStorageSync('id')
					},
					success: (res) => {
						this.list = res.data.data;
						console.log(this.list)
						// console.log(this.list[3].图片);
					}

				})
			},
			userDelete(index) {
				if (uni.getStorageSync('name') == "BOSS") {
					uni.request({
						url: "http://192.168.1.104:8000/admin/delete_user",
						method: "POST",
						data: {
							管理员id: uni.getStorageSync('id'),
							被删除id: this.list[index].员工id
						},
						success: (res) => {
							if (res.data.msg == "True") {
								console.log("删除成功");
								this.$u.toast("删除成功");
							}
							this.requestData();
						}

					})
				} else if (this.list[index].身份 == "管理员") {
					uni.$u.toast("您无法删除管理员!")
				} else {
					uni.request({
						url: "http://192.168.1.104:8000/admin/delete_user",
						method: "POST",
						data: {
							管理员id: uni.getStorageSync('id'),
							被删除id: this.list[index].员工id
						},
						success: (res) => {
							if (res.data.msg == "True") {
								console.log("删除成功");
								this.$u.toast("删除成功");
							}
							this.requestData();
						}

					})
				}


			},
			judgeStatus(url, id, index) {
				if (uni.getStorageSync('name') == "BOSS") {
					this.gotoEdit(url, id)
				} else if (this.list[index].身份 == "管理员") {
					uni.$u.toast("您无法修改管理员!")
					// location.reload()
				} else {
					this.gotoEdit(url, id)
				}
			},
			gotoEdit(url, id) {
				console.log(id)
				uni.navigateTo({
					url: url + "?id=" + id
				})
			}
		}
	};
</script>

<style scoped lang="scss">
	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		font-size: 32rpx;
		color: #333;
		padding: 20rpx 10rpx;
	}

	.u-body-item image {
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}

	.wrap {
		padding: 24rpx;
	}

	.u-row {
		margin: 40rpx 0;
	}

	.demo-layout {
		height: 80rpx;
		border-radius: 8rpx;
	}

	.bg-purple {
		background: #d3dce6;
	}

	.bg-purple-light {
		background: #e5e9f2;
	}

	.bg-purple-dark {
		background: #99a9bf;
	}

	.head_content {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 60rpx;
	}
</style>
