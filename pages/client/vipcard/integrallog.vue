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
					<navigator open-type="redirect" url="/pages/client/vipcard/integraldh">
					<view class="btn-integral">积分兑换</view>
					</navigator>
				</view>
			</view>
		</view>
		<view class="integral-log-main">
			<view class="pd16_15 bd-bottom flex alcenter">
				<text class="iconfont iconicon_integral_des ft20 cl-main"></text>
				<text class="ml10 ft16 ftw600 cl-main">积分明细</text>
			</view>
			<view class="pd16_15" v-if="listData.length>0">
				<block v-for="(item,index) in listData" :key="index">
				<view class="pb16 mb16 bd-bottom flex alcenter space">
					<view>
						<view class="ft14 cl-main">{{item.memo}}</view>
						<view class="mt8 ft12 cl-notice">{{item.createtime}}</view>
					</view>
					<view class="ft18 ftw600 cl-main">{{item.jzt}}{{item.score}}</view>
				</view>
				</block>
				<uni-load-more :status="status" :content-text="contentText" />
			</view>
			
			<com-empty v-else title="暂无积分记录"></com-empty>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
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
			this.status = 'more';
			this.getList();
		},
		onLoad(){
			this.ongrzlTap()
			this.getList();
		},
		methods:{
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
			getList() {
				let data = {
					//column: 'id,post_id,title,author_name,cover,published_at' //需要的字段名
				};
				var limit=10;
				if (this.last_id>0) {
					//说明已有数据，目前处于上拉加载
					this.status = 'loading';
					data.offset = this.last_id*limit;
					data._ = new Date().getTime() + '';
				}
				data.limit=limit		
				data.token = uni.getStorageSync("userinfo").token;
				uni.request({
					url: this.configs.webUrl+'/api/user/userscoreinfo',
					data: data,
					success: data => {
						console.log(data.data)
						if (data.data.total>0) {
							let list = data.data.rows;
							this.listData = this.reload ? list : this.listData.concat(list);
							this.reload = false;
							this.last_id = this.last_id+1;
							if(data.data.total<this.last_id*limit){
								this.status = '';
							}
						}else{
							this.contentText.contentdown='没有数据'
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
		width: 192rpx;
		height: 80rpx;
		background: #FFFFFF;
		box-shadow: 0rpx 8rpx 32rpx 0rpx rgba(0, 0, 0, 0.04);
		border-radius: 40rpx;
		text-align: center;
		line-height: 80rpx;
		color:#6687EF;
		font-size: 28rpx;
		font-weight: 500;
	}
	.integral-log-main{
		background: #FFFFFF;
		min-height: 1400rpx;
		position: relative;
		margin-top: -32rpx;
		border-radius: 32rpx 32rpx 0rpx 0rpx;
	}
</style>