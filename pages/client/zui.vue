<template>
	<view>
		<!-- 标题栏和状态栏占位符 -->
		<!-- <view class="titleNview-placing"> </view> -->
		<siginIn :sign_list="sign_list" :qdjl="qdjl" :tdsy="tdsy"  @cksigin="cksigin" @click="getsigin"></siginIn>
		<view class="pd16_15 mt16">
			<view class="pd16_15" style="box-shadow: 0px 0px 20upx 0px rgba(0,0,0,0.2);  border-radius: 20upx;">
				<view>
					<view class="flex alcenter space">
						<view class="flex alcenter">
							<!-- <image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[0]"></image> -->
							<text class="ft16 ftw600 cl-main">最近观看</text>
						</view>
						<navigator url="/pages/client/tuan/zjgk">
						<view class="ft14 cl-notice">更多</view>
						</navigator>
					</view>
					<view class="mt16 flex space">
						<block v-for="(value,key) in newsa" :key="key" v-if="key<3">
						<view class="box pb10" style="width:31%; position: relative; border-radius:20rpx;" @click="detail(value.vid,value.mid)">
							<!-- <view class="btn-mini" style="position: absolute; top: 20upx; right: 20upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx; z-index: 1;" :style="getBtnStyle">{{value.lxname}}</view> -->
							<image class="integral-mall-goods" mode="aspectFill" :src="value.img"></image>
							<view class="mt8 plr10 ft14 ftw400 text-center text-over cl-main">{{value.vname}}</view>
						</view>
						</block>	
				
					</view>
				</view>	
				<view class="mt24">
					<view class="flex alcenter space">
						<view class="flex alcenter">
							<!-- <image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[0]"></image> -->
							<text class="ft16 ftw600 cl-main">我的追剧</text>
						</view>
						<navigator url="/pages/client/tuan/zjjl">
						<view class="ft14 cl-notice">更多</view>
						</navigator>
					</view>
					<view class="mt16">
						<block v-for="(value,key) in record" :key="key">
						<view @click="detail(value.vid,value.mid)" style="position: relative;" class="flex alcenter mb16">
							<image mode="aspectFill" class="tuan-product-l" :src="value.img"></image>
							<!-- <view class="btn-mini" style="position: absolute; top: 40upx; left: 280upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx;" :style="getBtnStyle">{{value.lxname}}</view> -->
							<view class="tuan-product-r pl15">
								<view class="ft16 ftw600 cl-main text-over2">{{value.vname}}</view>
								<view class="mt16">
									<text class="ft14 cl-orange">{{value.dqname}}</text>
								</view>
								<view class="mt16 flex space alcenter">
									<view class="ft14 cl-notice">{{value.mname}}</view>
									<view class="btn-vip-adviser ml15">继续观看</view>
								</view>
							</view>
						</view>
						</block>
					</view>
				</view>	
			</view>
			
		</view>
	</view>
</template>
<script>
	import siginIn from '@/components/fanxiao-sign/sigin-in.vue';
	export default{
		components: {
		    siginIn
		},
		data(){
			return {
				qdjl:0,
				tdsy:0,
				navLock:false,
				datasa:[],
				showdyxx:true,
				dataconfig:[],
				banners:[],
				datainfo:[],
				dataindex:[],
				newsa:[],
				record:[],
				mbgColor:this.$mbgColor,
				sign_list: []
			}
		},
		computed:{
			
		},
		onPageScroll(e){
			
		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		onLoad(e){
		},
		onShow() {
			this.getList();
			let this_=this
			console.log(this.dataindex)
		},
		methods:{
			cksigin(){
				this.getList();
			},
			getsigin(e) {
			    console.log('已点击签到------');
			},
			getList() {
				let this_=this
				let data = {};
				if(uni.getStorageSync("userinfo")){
					data.uid = uni.getStorageSync("userinfo").id;
				}
				uni.request({
					url: this.configs.webUrl+'/api/video/zuiju',
					data: data,
					success: data => {
						this.newsa=data.data.new
						this.record=data.data.record
						this.sign_list=data.data.getweek
						this.qdjl=data.data.qdjl
						this.tdsy=data.data.tdsy
					},
					fail: (data, code) => {
					}
				});
			},
			saoma(){
				//#ifdef APP-PLUS  
				uni.scanCode({
				    success: function (res) {
						if(res.result.indexOf("uid") != -1){
							var obj = JSON.parse(res.result); 
							if(obj.uid){
								uni.navigateTo({
									url:'/pages/login/reg?uid='+obj.uid
								})
							}	
						}else{
							uni.showToast({ title: res.result,icon:"none" });	
						}	
				    }
				});
				//#endif
				
			},
			detail(vid,mid){
				var fxpid=1
				if(uni.getStorageSync("userinfo")){
					 fxpid=uni.getStorageSync("userinfo").id
				}
				uni.navigateTo({
					url:'/pages/client/tuan/detail?vid='+vid+'&mid='+mid+'&fxpid='+fxpid
				})
			},
			linkTo(e){
				if(this.isLogin == false){
					this.showLogin = true;
				}else{
					let link = e.currentTarget.dataset.link;
					uni.navigateTo({
						url:link
					})
				}
			},
			
			exchange(e){
				if(this.isLogin == false){
					this.showLogin = true;
				}else{
					let id = e.currentTarget.dataset.id;
					uni.navigateTo({
						url:'/pages/client/integral/exchange?id='+id
					})
				}
			},
		},
	}
</script>

<style>
	.home-header{
		height: 300rpx;
		width: 100%;
		position: relative;
		border-radius: 0rpx 0rpx 48rpx 48rpx;
	}
	.home-main{
		width: 100%;
		position: relative;
		margin-top: -156rpx;
		padding: 0 30rpx;
	}
	.home-mendian{
		width: 100%;
		height: 84rpx;
		background:rgba(255,255,255,0.1);
		border-radius: 42rpx;
	}
	.integral-mall-header{
		position: relative;
		height: 320rpx;
	}
	.integral-mall-header .bg{
		width: 100%;
		height: 320rpx;
	}
	.integral-mall-header .main{
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 320rpx;
	}
	.swiper-integral{
		height: 32rpx;
		width: 100%;
	}
	.integral-mall-main{
		position: relative;
		/* margin-top: -104rpx; */
	}
	.integal-mall-menu{
		width: 100%;
		height: 190rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
	}
	.integral-tuan-l{
		width: 100%;
		height: 280rpx;
		background: #f2f2f2;
		border-radius: 16rpx;
	}
	
	
	.integral-mall-coupon{
		background: #FFFFFF;
		position: relative;
		border-radius: 16rpx;
		overflow: hidden;
	}
	.integral-mall-coupon  .top{
		padding: 0rpx 0rpx 24rpx 0rpx;
		border-bottom: 2rpx dashed #FEC675;
	}
	.integral-mall-coupon  .y-l,.integral-mall-coupon  .y-r{
		width: 20rpx;
		height: 20rpx;
		border-radius: 10rpx;
		background: #F5F6FA;
		position: absolute;
		z-index: 2;
		top: 284rpx;
	}
	.integral-mall-coupon  .y-l{
		left: -10rpx;
	}
	.integral-mall-coupon  .y-r{
		right: -10rpx;
	}
	.integral-mall-coupon   .coupon-value{
		width: 100%;
	}
	.integral-mall-coupon  .coupon-value image{
		width: 100%;
	}
	.integral-mall-coupon  .coupon-value .num{
		width: 100%;
		height: 64rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		left: 0;
		top: 0;
	}
	.integral-mall-goods{
		width: 100%;
		height: 280rpx;
		background: #F2F2F2;
		border-radius: 16upx;
	}
	.titleNview-placing {
		height: var(--status-bar-height);
		padding-top: 44px;
		box-sizing: content-box;
	}
	.tuan-product-l{
		width: 150rpx;
		height: 200rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.tuan-product-r{
		width: calc(100% - 150rpx);
	}
	.btn-vip-adviser{
		width: 150rpx;
		height: 60rpx;
		border-radius: 20rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 28rpx;
		/* font-weight: 600; */
		background: #FF6600;
		color:#FFFFFF;
	}
</style>