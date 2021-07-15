<template>

	<view class="">
		<!-- <u-modal v-model="showID" title="请输入ID" :title-style="{color: 'red'}" @confirm="confirmID">
			<view class="slot" :id="id">
				<u-input v-model="id" type="text" border="border"></u-input>
			</view>
		</u-modal> -->
		<u-modal v-model="showTitle" title="请输入菜名" :title-style="{color: 'red'}" @confirm="confirmTitle">
			<view class="slot" :title="title">
				<u-input v-model="title" type="text" border="border" placeholder="菜名"></u-input>
			</view>
		</u-modal>
		<u-modal v-model="showPrice" title="请输入价格" :title-style="{color: 'red'}" @confirm="confirmPrice">
			<view class="slot" :price="price">
				<u-input v-model="price" type="text" border="border" placeholder="价格"></u-input>
			</view>
		</u-modal>
		<u-modal v-model="showRecommend" title="是否为推荐菜" :title-style="{color: 'red'}" @confirm="confirmRecommend">
			<view class="slot" :recommend="recommend">
				<u-input v-model="newRecommend" type="text" border="border" placeholder="是否为推荐菜"></u-input>
			</view>
		</u-modal>
		</u-modal>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16">
			<u-col span="2">
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="8">
				<view style="text-align: center; font-size: 80rpx;">
					增加菜品
				</view>
			</u-col>
			<u-col span="2">
			</u-col>
		</u-row>
		<view style="height: 100rpx;"></view>
		<!--上传图片-->
		<view class="head_content">
			<u-upload :action="action" :auto-upload="true" @on-remove="remove" @on-success="success" max-count="1">
			</u-upload>
		</view>

		<view style="height: 60rpx;"></view>
		<!-- 标题栏-->
		<view style="height: 60rpx;"></view>
		<u-row gutter="16">
			<u-col span="3" :id="id">
				<u-button size="default" @click="EditID" style="font-size: 100rpx; height: 150rpx;">ID</u-button>
			</u-col>
			<!-- 之后需要对字体居中、大小进行修改-->
			<u-col span="3" :title="title">
				<u-button size="default" @click="EditTitle" style="font-size: 100rpx; height: 150rpx;"
					placeholder="菜品名">{{title}}</u-button>
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
	export default {
		data() {
			return {
				id: 0,
				title: '菜名',
				// ssTitle: '',
				price: 0,
				images: "",
				show: false,
				intro: "",
				recommend: "",
				detail: "",
				showID: false,
				showTitle: false,
				showPrice: false,
				showRecommend: false,
				newTitle: "",
				newPrice: "",
				newRecommend: "",
				border: true,
				confirmAgain: false,
				action: 'http://192.168.1.104:8000/admin/upload_pic'
			}
		},
		methods: {
			EditID() {
				// this.showID = true;
			},
			EditTitle() {
				this.showTitle = true;
			},
			EditPrice() {
				this.showPrice = true;
			},
			EditRecommend() {
				this.showRecommend = true;
			},
			confirmID() {},
			confirmTitle() {},
			confirmPrice() {},
			confirmRecommend() {
				if (this.newRecommend == "推荐菜" || this.newRecommend == "特色菜") {
					this.recommend = this.newRecommend;
				} else {
					this.recommend = "";
					uni.$u.toast("请输入推荐菜或特色菜！")
				}
			},
			cancel() {
				console.log("取消成功")
				uni.switchTab({
					url: './food'
				})
			},
			confirm() {
				uni.request({
					url: "http://192.168.1.104:8000/admin/add",
					method: "POST",
					data: {
						id: uni.getStorageSync('id'),
						菜名: this.title,
						价格: this.price,
						是否推荐: this.recommend,
						简介: this.intro,
						图片: this.images
					},
					success: (res) => {
						if (res.data.msg == "True") {
							console.log("增加菜品成功")
							uni.$u.toast("增加菜品成功")
							uni.switchTab({
								url: "./food"
							})
						}else{
							uni.$u.toast("请输入所有内容")
						}

					}
				})

			},
			remove() {
				console.log("图片移出")
			},
			success(data, index, lists, name) {
				console.log(data)
				this.images = data.msg;
				uni.switchTab({
					url: "./user"
				})
			},
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

	.head_content {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 60rpx;
	}
</style>
