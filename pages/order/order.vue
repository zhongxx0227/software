<template>
	<view class="" slot="head">
		<view style="height: 100rpx;"></view>
		<view style="align-items: center;">
			<u-row gutter="16">
				<u-col span="2">
				</u-col>
				<!-- 之后需要对字体居中、大小进行修改-->
				<u-col span="8">
					<view style="text-align: center; font-size: 80rpx;">
						订单详情
					</view>
				</u-col>
				<u-col span="2">
				</u-col>
			</u-row>
		</view>
		<!-- <u-line color="black" /> -->
		<view style="height: 100rpx;"></view>
		<u-line color="black" />
		<view style="align-items: center;">
			<u-row gutter="16">
				<u-col span="2">
					<view class="demo-layout bg-purple">
						<view style="text-align: center; font-size: 60rpx;">
							桌号
						</view>
					</view>
				</u-col>
				<!-- 之后需要对字体居中、大小进行修改-->
				<u-col span="2">
					<view class="demo-layout bg-purple">
						<view style="text-align: center; font-size: 60rpx;">
							状态
						</view>
					</view>
				</u-col>
				<u-col span="4">
					<view class="demo-layout bg-purple">
						<view style="text-align: center; font-size: 60rpx;">
							下单时间
						</view>
					</view>
				</u-col>
				<u-col span="2">
					<view class="demo-layout bg-purple">
						<view style="text-align: center; font-size: 60rpx;">
							金额
						</view>
					</view>
				</u-col>
				<u-col span="2">
					<view class="demo-layout bg-purple">
						<view style="text-align: center; font-size: 60rpx;">
							操作
						</view>
					</view>
				</u-col>
			</u-row>
		</view>
		<view style="height: 100rpx;"></view>
		<view v-for="(item,index) in list">
			<u-row gutter="16">
				<u-col span="2">
					<view class="bg-grey">
						<view style="text-align: center; font-size: 60rpx;">
							{{item.桌号}}
						</view>
					</view>
				</u-col>
				<!-- 之后需要对字体居中、大小进行修改-->
				<u-col span="2">
					<view class="bg-grey">
						<view style="text-align: center; font-size: 60rpx;">
							{{item.订单状态}}
						</view>
					</view>
				</u-col>
				<u-col span="4">
					<view class="bg-grey">
						<view style="text-align: center; font-size: 60rpx;">
							{{item.下单时间}}
						</view>
					</view>
				</u-col>
				<u-col span="2">
					<view class="bg-grey">
						<view style="text-align: center; font-size: 60rpx;">
							{{item.金额}}
						</view>
					</view>
				</u-col>
				<u-col span="2">
					<view class="bg-grey">
						<u-button style="color: #19BE6B;" @click="click(index)">
							<text style="font-size: 60rpx;">查看</text>
						</u-button>
					</view>
				</u-col>
			</u-row>
			<view style="height: 100rpx;"></view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: []
			}
		},
		onShow() {
			uni.request({
				url: "http://192.168.1.104:8000/admin/show",
				method: "POST",
				data: {
					id: uni.getStorageSync('id'),
				},
				success: (res) => {
					console.log("展示订单连接成功")
					this.list = res.data.data;
					console.log(this.list)
				}
			})
		},
		methods: {
			click(index) {
				// console.log(this.list[index])
				console.log(index)
				if (this.list[index].订单状态 == "正在用餐") {
					this.gotoOrderDetail("./orderDetail", JSON.stringify(this.list[index]))
				} else if ((this.list[index]).订单状态 == "等待支付") {
					this.gotoPay("./pay", JSON.stringify(this.list[index]))
				}
			},
			gotoOrderDetail(url, id) {
				uni.navigateTo({
					url: url + "?id=" + id
				});
			},
			gotoPay(url, id) {
				uni.navigateTo({
					url: url + "?id=" + id
				});
			}
		}
	}
</script>

<style>
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
</style>
