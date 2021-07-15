<template>
	<view class="thispage">
		<u-top-tips ref="uTips"></u-top-tips>
		<u-modal v-model="show" :content="content" :show-cancel-button='true' @confirm="logout()"></u-modal>
		
		<!-- 信息栏目 -->
		<view class="status_view">
			<u-row gutter="16">
				<u-col span="3">
					<u-image width="90%" height="300rpx" border-radius="40" :src="user.src"></u-image>
				</u-col>
				<!-- 之后需要对字体居中、大小进行修改-->
				<u-col span="4">
					<view class="name-view">
						{{user.name}}
					</view>
					<view class="job-view">
						职位: {{user.job}}
					</view>
				</u-col>
			</u-row>
		</view>
		
		<u-gap height="30" bg-color="#f5f5f5"></u-gap>
		
		<!-- 操作栏 -->
		<view class="operation">
				<u-row>
					<u-col sapn="12">
						<view class="logout" @click="click">
							退出登录
						</view>
					</u-col>
				</u-row>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				baseURL:"http://192.168.1.104:8000/admin",
				user:{
					name:"",
					id:0,
					job:-1,
					src:"",
				},
				show:false,
				content:"是否确认退出",
			}
		},
		methods: {
			click(){
				this.show=true;
			},
			goToLogin(){
				uni.navigateTo({
					url:"/pages/login/login",
					success(res) {
					console.log(res);
					},
					fail(err) {
					console.log(err);
					}
				})
			},
			logout(){
				uni.request({
					url: this.baseURL+ "/logout", 
					method: 'POST',
					data: {
						id:uni.getStorageSync('id'),
					},
					dataType: 'json',
					success: (res) => {
						console.log("successful");
						console.log(res.data);
						//添加成功通知栏提示
						if (res.data.msg == "WORKING") {
							this.$refs.uTips.show({
								title: '订单进行中，无法退出',
								type: 'error',
								duration: '2300'
							})
						}
						else if(res.data.msg == "False"){
							this.$refs.uTips.show({
								title: '出现异常，无法退出',
								type: 'error',
								duration: '2300'
							})
						}
						else{
							this.$refs.uTips.show({
								title: '成功退出',
								type: 'success',
								duration: '2300'
							})
							this.goToLogin();
						}
					},
				});
			},
			onShow(){
				this.user.id = uni.getStorageSync('id');
				this.user.name = uni.getStorageSync('name');
				this.user.job = uni.getStorageSync('job');
				this.user.src=uni.getStorageSync('src');
				console.log(this.user.name)
				console.log(this.user.src)
				if(this.user.job==0){
					this.user.job="后厨";
				}else if(this.user.job==1){
					this.user.job="服务员";
				}else if(this.user.job==2){
					this.user.job="管理员";
				}else if(this.user.job==3){
					this.user.job="Boss";
				}else{
					this.user.job="未知";
				}
			}
		}
	}
</script>

<style>
	.thispage{
		height: 100vh;
		background-color: #f5f5f5;
	}
	.status_view{
		height: 15vh;
		padding-left: 20rpx;
		padding-top: 60rpx;
		margin-bottom: 30rpx;
		background-color: #FFFFFF;
	}
	.name-view{
		font-size: 100rpx;
		font-weight: 500rpx;
		padding-bottom: 10rpx;
		margin-bottom: 30rpx;
	}
	.job-view{
		text-align: left;
		font-size: 75rpx;
	}
	.operation{
		height: 150rpx;
		background-color: #ffffff;
	}
	.logout{
		font-size: 70rpx;
		height: 150rpx;
		padding-top: 30rpx;
		padding-left: 30rpx;
	}
</style>
