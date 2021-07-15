<template>

	<view class="">
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="8">
				<view style="text-align: center; font-size: 80rpx;">
					菜品详情
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
		<view style="height: 100rpx;"></view>
		<view class="orgimg">
			<u-image width="80%" height="1000rpx" :src="图片" mode="aspectFit"></u-image>
		</view>

		<!-- 标题栏-->
		<view style="height: 60rpx;"></view>
		<u-row gutter="16">
			<u-col span="3" :id="菜品id">
				<u-button size="default" style="font-size: 100rpx; height: 150rpx;">ID: {{菜品id}}</u-button>
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="3" :title="菜品名">
				<u-button size="default" style="font-size: 100rpx; height: 150rpx;">{{菜品名}}</u-button>
			</u-col>
			<u-col span="3 " :price="价格">
				<u-button size="default" style="font-size: 100rpx; height: 150rpx;">¥{{价格}}</u-button>
			</u-col>
			<u-col span="3" :recommend="是否推荐">
				<u-button size="default" style="font-size: 100rpx; height: 150rpx;">{{是否推荐}}</u-button>
			</u-col>
		</u-row>
		<view style="height: 60rpx;"></view>
		<u-card title="菜品简介" style="font-size: 80rpx;" :intro="简介" title-color="blue" title-size="100rpx">
			<view class="" slot="body">
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title u-line-2" style="height: 150rpx; font-size: 60rpx;">
						{{简介}}
					</view>
				</view>
			</view>
		</u-card>
		<!-- <u-card title="详细介绍" style="font-size: 80rpx;" :intro="详情" title-color="blue" title-size="100rpx">
			<view class="" slot="body">
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title u-line-2" style="height: 150rpx; font-size: 60rpx;">
						{{详情}}
					</view>
				</view>
			</view>
		</u-card> -->
		<view style="height: 60rpx;"></view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				菜品id: 1,
				菜品名: '',
				价格: "",
				图片: "",
				show: false,
				简介: "",
				是否推荐: "",
			}
		},
		onLoad: function(options) {
			let list = JSON.parse(options.id)
			console.log(list)
			this.菜品id = list.菜品id
			console.log(this.菜品id)
		},
		onShow() {
			uni.request({
				url:"http://192.168.1.104:8000/admin/query",
				method:"POST",
				data:{
					id: uni.getStorageSync('id'),
					菜品id: this.菜品id
				},
				success: (res) => {
					console.log(res)
					this.id = res.data.data.菜品id;
					this.菜品名 = res.data.data.菜名;
					this.价格 = res.data.data.价格;
					this.图片 = res.data.data.图片;
					this.简介 = res.data.data.简介;
					this.是否推荐 = res.data.data.是否推荐;
				}
			})
		}

	}
</script>

<style>
	.orgimg {
		width: 100%;

		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		font-size: 50rpx;
		color: #333;
		padding: 20rpx 10rpx;
	}

	.u-body-item image {
		width: 100rpx;
		flex: 0 0 100rpx;
		height: 100rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}
</style>
