<template>

	<view>
		<u-card padding="10">
			<view class="" slot="head">
				<view style="align-items: center;">
					<u-row gutter="16">
						<u-col span="2">
							<!-- <u-button size="default">返回</u-button> -->
						</u-col>
						<!-- 之后需要对字体居中、大小进行修改-->
						<u-col span="8">
							<view style="text-align: center; font-size: 80rpx;">
								菜品管理
							</view>
						</u-col>
						<u-col span="2">
							<u-button size="default" @click="gotoAdd('./foodAdd')"
								style="font-size: 75rpx; height: 150rpx;" type="success">增加菜品</u-button>
						</u-col>
					</u-row>
				</view>
			</view>
		</u-card>
		<u-swipe-action :show="item.show" style="font-size: ;" :index="index" v-for="(item, index) in list"
			:key="item.id" @click="click" @open="open" :options="options">
			<view class="item u-border-bottom">
				<image mode="aspectFill" :src="item.图片" />
				<!-- <view style="width: 50rpx;"></view> -->
				<!-- 此层wrap在此为必写的，否则可能会出现标题定位错误 -->
				<view class="title-wrap">
					<u-tag :text=item.菜品名称
						style="font-size: 100rpx; height: 200rpx; display: flex; justify-content: center;"
						bg-color="#FDF5E6" color="#000000" border-color="#FDF5E6"></u-tag>
				</view>
				<!-- <view style="width: 800rpx;"></view> -->

				<view class="price-wrap">
					<view>
						<text
							style="background-color: #FDF5E6;  width: 400rpx; font-size: 100rpx; height: 200rpx; display: flex; justify-content: center; align-items: center; margin-right: 100rpx;">¥{{item.菜品价格}}</text>
					</view>

					<!-- <u-tag :text= "item.菜品价格" style=" width: 1000rpx; font-size: 100rpx; height: 200rpx; display: flex; justify-content: flex-end; align-items: center; margin-right: 100rpx;" bg-color="#FDF5E6" color="#000000"></u-tag> -->
				</view>
			</view>
		</u-swipe-action>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				disabled: false,
				btnWidth: 180,
				show: false,
				options: [{
						text: '修改',
						style: {
							backgroundColor: '#007aff'
						}
					},
					//加入模态框
					{
						text: '删除',
						style: {
							backgroundColor: '#dd524d'
						}
					},
					{
						text: '查看详情',
						style: {
							backgroundColor: '#005500'
						}
					}
				]
			};
		},
		onShow() {
			uni.request({
				url: "http://192.168.1.104:8000/admin/query_menu",
				method: "POST",
				data: {
					id: uni.getStorageSync('id'),
				},
				success: (res) => {
					this.list = res.data.data;
					console.log(this.list)
				}
			})
		},
		onLoad() {
			// location.reload();
			// this.$router.go(0);
		},
		methods: {
			//在这里添加点击的事件
			click(index, index1) {
				// location.reload();
				if (index1 == 1) {
					uni.request({
						url: "http://192.168.1.104:8000/admin/delete",
						method: "POST",
						data: {
							id: uni.getStorageSync('id'),
							菜品id: this.list[index].菜品id
						},
						success: (res) => {
							console.log("删除菜品成功")
							console.log(this.list[index].菜品名称)
							this.list.splice(index, 1);
							// this.$u.toast("删除了"+this.list[index].菜品名称);

						}
					})
					// this.list[index].show = false;
				} else if (index1 == 2) {
					this.list[index].show = false;
					this.gotoDetails("./foodDetails", JSON.stringify(this.list[index]))
					console.log("查看详情跳转成功")
					// this.$u.toast(`收藏成功`);
				} else {
					this.list[index].show = false;
					// console.log("修改跳转成功")
					this.gotoXiugai("./foodXiugai", JSON.stringify(this.list[index]));
				}
			},
			// 如果打开一个的时候，不需要关闭其他，则无需实现本方法
			open(index) {
				// 先将正在被操作的swipeAction标记为打开状态，否则由于props的特性限制，
				// 原本为'false'，再次设置为'false'会无效
				this.list[index].show = true;
				this.list.map((val, idx) => {
					if (index != idx) this.list[idx].show = false;
				})
			},
			gotoXiugai(url, id) {
				console.log(id)
				uni.navigateTo({
					url: url + "?id=" + id

				})
			},
			gotoDetails(url, id) {
				console.log(id)
				uni.navigateTo({
					url: url + "?id=" + id

				})
			},
			gotoAdd(url) {
				uni.navigateTo({
					url: url

				})
			},
			beforeCreate: function() {
				console.log('beforeCreate');
			}
		}
	};
</script>

<style lang="scss" scoped>
	.item {
		display: flex;
		padding: 80rpx;
	}

	image {
		width: 300rpx;
		flex: 0 0 300rpx;
		height: 300rpx;
		margin-right: 20rpx;
		border-radius: 12rpx;
	}

	.title {
		text-align: left;
		font-size: 60rpx;
		color: $u-content-color;
		margin-top: 20rpx;
	}

	.price {
		text-align: right;
		font-size: 60rpx;
		color: $u-content-color;
		margin-top: 20rpx;
	}

	.title-wrap {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 100rpx;
		width: 800rpx;
		margin-left: 50rpx;

	}

	.price-wrap {
		display: flex;
		align-items: center;
		text-align: center;
		margin-left: 200rpx;
		justify-content: center;
		font-size: 100rpx;
		width: 500rpx;
	}
</style>
