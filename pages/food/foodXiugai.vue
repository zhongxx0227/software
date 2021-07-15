<template>

	<view class="">
		<u-modal v-model="showTitle" title="请输入菜名" :title-style="{color: 'red'}" @confirm="confirmTitle"
			@cancel="cancel" style="font-size: 50rpx;">
			<view class="slot" :title="title">
				<u-input v-model="newTitle" type="text" border="border" :placeholder="title"></u-input>
			</view>
		</u-modal>
		<u-modal v-model="showPrice" title="请输入价格" :title-style="{color: 'red'}" @confirm="confirmPrice"
			@cancel="cancel">
			<view class="slot" :title="title">
				<u-input v-model="newPrice" type="text" border="border"></u-input>
			</view>
		</u-modal>
		<u-modal v-model="showRecommend" title="是否为推荐菜" :title-style="{color: 'red'}" @confirm="confirmRecommend"
			@cancel="cancel">
			<view class="slot" :title="title">
				<u-input v-model="newRecommend" type="text" border="border"></u-input>
			</view>
		</u-modal>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="8">
				<view style="text-align: center; font-size: 80rpx;">
					菜品修改
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
		<view style="height: 100rpx;"></view>
		<view class="orgimg">
			<u-image width="80%" height="1000rpx" :src="images" mode="aspectFit"></u-image>
		</view>
		<view class="head_content">
			<u-upload :action="action" :auto-upload="true" @on-remove="remove" @on-success="success">
			</u-upload>
		</view>
		

		<!-- 标题栏-->
		<view style="height: 60rpx;"></view>
		<u-row gutter="16">
			<u-col span="3" :id="id">
				<u-button size="default" style="font-size: 100rpx; height: 150rpx;">ID: {{id}}</u-button>
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="3" :title="title">
				<u-button size="default" @click="EditTitle" style="font-size: 100rpx; height: 150rpx;">{{title}}
				</u-button>
			</u-col>
			<u-col span="3 " :price="price">
				<u-button size="default" @click="EditPrice" style="font-size: 100rpx; height: 150rpx;">¥{{price}}
				</u-button>
			</u-col>
			<u-col span="3" :recommend="recommend">
				<u-button size="default" @click="EditRecommend" style="font-size: 100rpx; height: 150rpx;">{{recommend}}
				</u-button>
			</u-col>
		</u-row>

		<view style="height: 60rpx;"></view>
		<u-card title="菜品简介" style="font-size: 60rpx;" :intro="intro" title-color="blue" title-size="100rpx">
			<view class="" slot="body">
				<u-input v-model="intro" type="textarea" height="400rpx"></u-input>
			</view>
		</u-card>
		<u-card title="详细信息" style="font-size: 60rpx;" :detail="detail" title-color="blue" title-size="100rpx">
			<view class="" slot="body">
				<u-input v-model="detail" type="textarea" height="400rpx"></u-input>
			</view>
		</u-card>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="4">
				<view>
					<!-- <u-button @click="cancel" type="primary" style="font-size: 50rpx;" >取消</u-button> -->
					<u-button type="primary" @click="cancel" style="font-size: 80rpx; height: 120rpx;">取消</u-button>
				</view>
			</u-col>
			<u-col span="4">
				<view>
					<!-- <u-button @click="confirm" type="warning" style="font-size: 50rpx;">确认</u-button> -->
					<u-button type="success" @click="confirm" style="font-size: 80rpx; height: 120rpx;">确认</u-button>
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
				id: 1,
				title: '',
				price: 0,
				images: "",
				show: false,
				intro: "",
				recommend: "",
				detail: "",
				showTitle: false,
				showPrice: false,
				showRecommend: false,
				newTitle: "",
				newPrice: 0,
				newRecommend: "",
				border: true
			}
		},
		onLoad: function(options) {
			let list = JSON.parse(options.id)
			console.log(list)
			this.id = list.菜品id
			this.images = list.图片
			this.recommend = list.是否推荐
			this.title = list.菜品名称
			this.intro = list.简介
			this.price = list.菜品价格
			console.log(this.id)
		},
		methods: {
			EditTitle() {
				this.showTitle = true;
			},
			EditPrice() {
				this.showPrice = true;
			},
			EditRecommend() {
				this.showRecommend = true;
			},
			confirmTitle() {
				this.title = this.newTitle;
			},
			confirmPrice() {
				this.price = this.newPrice;
			},
			confirmRecommend() {
				this.recommend = this.newRecommend;
			},
			cancel() {
				uni.switchTab({
					url: "./food"
				})
			},
			confirm() {
				uni.request({
					url: "http://192.168.1.104:8000/admin/modify",
					method: "POST",
					data: {
						菜品id: this.id,
						id: uni.getStorageSync('id'),
						菜名: this.title,
						价格: this.price,
						是否推荐: this.recommend,
						简介: this.intro,
						图片: this.images
					},
					success: (res) => {
						console.log("发送成功")
					}
				})
				uni.switchTab({
					url: "./food"
				})
			}
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
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}

	.slot-content {
		font-size: 28rpx;
		color: $u-content-color;
		padding-left: 30rpx;
	}
</style>
