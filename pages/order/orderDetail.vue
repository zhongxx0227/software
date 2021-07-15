<template>
	<view>
		<view style="height: 60rpx;"></view>
		<u-card padding="10">
			<view class="" slot="head">
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
			</view>
		</u-card>
		<view style="height: 100rpx;"></view>
		<u-row gutter="16" :桌号="桌号" :下单时间="下单时间" :人数="人数">
			<u-col span="3">
				<view class="head_content">
					桌号:{{桌号}}
				</view>
			</u-col>
			<u-col span="3">
				<view class="head_content">
					人数:{{人数}}
				</view>
			</u-col>
			<u-col span="6">
				<view class="head_content">
					下单时间:{{下单时间}}
				</view>
			</u-col>
		</u-row>
		<view style="height: 60rpx;"></view>
		<u-card padding="10">
			<view class="" slot="body">
				<view style="align-items: center;">
					<u-row gutter="16">
						<u-col span="4">
							<view class="head_content">菜名</view>
						</u-col>
						<!-- 之后需要对字体居中、大小进行修改-->
						<u-col span="4">
							<view class="head_content">数量</view>
						</u-col>
						<u-col span="4">
							<view class="head_content">价格</view>
						</u-col>
					</u-row>
				</view>
			</view>
		</u-card>
		<view v-for="(item,index) in list">
			<u-card padding="10">
				<view class="" slot="body">
					<view style="align-items: center;">
						<u-row gutter="16">
							<u-col span="4">
								<view class="head_content">{{item.菜品名称}}</view>
							</u-col>
							<!-- 之后需要对字体居中、大小进行修改-->
							<u-col span="4">
								<view class="head_content">{{item.数量}}</view>
							</u-col>
							<u-col span="4">
								<view class="head_content">¥{{item.价格}}</view>
							</u-col>
						</u-row>
					</view>
				</view>
			</u-card>
		</view>
		<view style="height: 100rpx;"></view>
		<u-card padding="10">
			<view class="" slot="body">
				<view style="align-items: center;">
					<u-row gutter="16">
						<u-col span="8">
							<view class="head_content":totalFood="菜品总数">总计:{{菜品总数}} </view>
						</u-col>
						<!-- 之后需要对字体居中、大小进行修改-->
						<u-col span="8">
							<view class="head_content" :totalAmount="金额">总金额:{{金额}}</view>
						</u-col>
					</u-row>
				</view>
			</view>
		</u-card>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				桌号: "",
				下单时间: "",
				金额: 0,
				人数: 0,
				菜品总数:0,
				list: []
			}
		},
		onLoad: function(options) {
			let transData = JSON.parse(options.id)
			// console.log(list)
			this.list = transData.订单内容
			this.桌号 = transData.桌号
			this.下单时间 = transData.下单时间
			this.金额 = transData.金额
			this.人数 = transData.人数
			this.菜品总数 = transData.菜品总数
			console.log(this.list)
		},
	}
</script>

<style>
	/* 居中标准代码 */
	.head_content {
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		font-size: 60rpx;
	}
</style>
