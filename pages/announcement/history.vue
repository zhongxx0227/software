<template>
	<view class="thispage">
		<view style="height: 200rpx; display: flex;justify-content: left; align-items: left;">
			<u-icon name="arrow-left" size="100rpx" style="display: flex; margin-left: 100rpx;"
				@click="back"></u-icon>
		</view>
		<u-top-tips ref="uTips1"></u-top-tips>
		<view class="noti_content">
			<u-card v-for="(item, index) in notis" class="mycard" :style="cardstyle">
				<view slot="head" class="head_content">
					<u-icon name="chat-fill" size='90' class='icon'></u-icon>
					<text class="title">{{item.标题}}</text>
					<text class="subtitle">{{item.时间}}</text>
				</view>
				<view slot="body" class="body_content">
					{{item.内容}}
				</view>
			</u-card>
		</view>
	</view>
</template>

<script>
	export default {
		onShow() {
			this.request_noti();
		},
		// 下拉刷新函数
		onPullDownRefresh() {
			console.log("触发了下拉刷新")
			setTimeout(() => {
				this.request_noti();
				uni.stopPullDownRefresh()
			}, 1000)
		},
		data() {
			return {
				notis: [],
				cardstyle: {
					'border-radius': '20px'
				}
			}
		},
		methods: {
			request_noti() {
				console.log("processing");
				uni.request({
					url: "http://192.168.1.104:8000/admin/broadcast",
					method: "POST",
					data: {
						id: uni.getStorageSync('id')
					},
					success: (res) => {
						console.log("successful");
						console.log(res.data.data);
						if (res.data.msg == "True") {
							// 赋值给本地
							this.notis = res.data.data;
						} else {
							this.$refs.uTips1.show({
								title: '通知请求失败',
								type: 'error',
								duration: '2300'
							})
						}
					},
				});
			},
			back() {
				uni.switchTab({
					url: "./announcement"
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.thispage {
		display: flex;
		height: 100vh;
		align-items: center;
		flex-direction: column;
		background-color: #f5f5f5;
	}

	.noti_content {
		width: 95%;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.mycard {
		width: 95vw;
	}

	.head_content {
		display: flex;
		justify-content: flex-start;
		flex-direction: row;
	}

	.icon {
		margin-right: 80rpx;
	}

	.title {
		font-size: 30px;
		padding-top: 8rpx;
	}

	.subtitle {
		font-size: 25px;
		padding-top: 8rpx;
		text-align: center;
		margin-left: 500px;
	}

	.body_content {
		font-size: 30px;
		// height: 150rpx;
		background-color: #ffffff;
		border-radius: 0 0 20px 20px;
	}

	.blank {
		height: 20px;
		background-color: #19BE6B;
	}
</style>
