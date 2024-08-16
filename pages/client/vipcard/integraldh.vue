<template>
	<view>
		<view class="integral-log-header">
			<image class="bg" :src="statics.integralLogHeader"></image>
			<view class="main pd24_20">
				<view class="flex alcenter space">
					<view>
						<view class="flex alcenter">
							<text class="ft14 cl-w">当前积分</text>
							<navigator open-type="redirect" url="/pages/client/integral/role">
								<text class="ml5 iconfont iconbtn_question cl-w ft20"></text>
							</navigator>
						</view>
						<view class="mt16 flex alcenter">
							<image :src="statics.integralImg01" style="width: 40rpx; height: 40rpx;"></image>
							<text class="ml10 ft20 cl-w ftw600">{{userinfo.score}}</text>
						</view>
					</view>
					<view class="btn-integral">
					<view class="ft14 mb10">兑换说明：</view>
					{{userinfo.config.jfdhsm}}
					</view>
	
				</view>
			</view>
		</view>
		<view class="integral-log-main pd16_15">
			<view class="ft18">兑换数量:</view>
			<input class="ft14 mt16" type="digit" placeholder="请输入兑换数量" style="border: 1px solid #ddd; height: 80upx; line-height: 80upx; border-radius: 16upx; padding: 10upx;" v-model="sum" />
		<button @click="duihuan()" class="btn-vip-money ft18 mt20" style="width: 80%; background: rgb(0, 198, 87); color: #ffffff; text-align: center; height: 100upx; line-height: 100upx;">提交兑换</button>
		</view>
		
	</view>
</template>

<script>
	export default{
		data(){
			return {
				sum: '',
				userinfo:[],
				listData: [],
				last_id: 0,
				reload: true,
				status: 'more',
				userinfo:[],
				contentText: {
					contentdown: '上拉加载更多',
					contentrefresh: '加载中',
					contentnomore: '没有数据了'
				}
			}
		},
		onReachBottom() {

		},
		onLoad(){
			this.ongrzlTap()
		},
		methods:{
			duihuan(){
				let data = {};
				data.token = uni.getStorageSync("userinfo").token;
				data.uid = uni.getStorageSync("userinfo").id;
				if(!this.sum){
					uni.showToast({
					  icon: 'none',
					  title: '请输入兑换数量'
					});
					return;
				}
				data.sum=this.sum
				uni.request({
					url: this.configs.webUrl+'/api/user/duihuan',
					data: data,
					success: res =>{
						if(res.data.code==1){
							this.ongrzlTap()
							uni.showToast({ title: res.data.msg,icon:"none" });
						}else{
							uni.showToast({ title: res.data.msg,icon:"none" });
						}
					},
					fail: (data, code) => {
						
					}
				});
			},
			async ongrzlTap(){
				let data = {};
				data.token = uni.getStorageSync("userinfo").token;
				data.uid = uni.getStorageSync("userinfo").id;
				uni.request({
					url: this.configs.webUrl+'/api/user/index',
					data: data,
					success: res =>{
						if(res.data.code==1){
							let ionfo=res.data.data
							this.userinfo=ionfo
						}else{
							uni.showToast({ title: res.data.msg,icon:"none" });
						}
					},
					fail: (data, code) => {
						//console.log('fail' + JSON.stringify(data));
					}
				});		
			},
			
		}
	}
</script>

<style>
	.integral-log-header{
		position: relative;
		height: 252rpx;
	}
	.integral-log-header .bg{
		width: 100%;
		height: 252rpx;
	}
	.integral-log-header .main{
		height: 252rpx;
		width: 100%;
		position: absolute;
		left: 0;
		top: 0;
	}
	.btn-integral{
		color:#ffffff;
		font-size: 24rpx;
	}
	.integral-log-main{
		background: #FFFFFF;
		min-height: 1400rpx;
		position: relative;
		margin-top: -32rpx;
		border-radius: 32rpx 32rpx 0rpx 0rpx;
	}
</style>