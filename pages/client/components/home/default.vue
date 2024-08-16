<template>
	<view>
		<view class="pd16_15" style="margin-bottom: 80upx;">	
			<view class="flex alcenter space" style="margin-bottom: 30upx;">
				<view class="flex alcenter">
					<image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[3]"></image>
					<text class="ft16 ftw600 cl-main ml15">最新热播</text>
				</view>
				<navigator url="/pages/client/tuan/ss">
				<view class="ft14 cl-notice">大家都在看的热播好剧</view>
				</navigator>
			</view>
			<view class="flex space" style="display:flex; flex-wrap:wrap;">
				<block v-for="(value,key) in datasa" :key="key">
				<view class="box pb10 mb15" style="width:31%; position: relative; border-radius:20rpx;" @click="detail(value.id,0)">
					<!-- <view class="btn-mini" style="position: absolute; top: 20upx; right: 20upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx; z-index: 1;" :style="getBtnStyle">{{value.lxname}}</view> -->
					<image class="integral-mall-goods" mode="aspectFill" :src="value.img"></image>
					<view class="mt8 plr10 ft14 ftw400 text-center text-over cl-main">{{value.name}}</view>
				</view>
				</block>
			</view>
		</view>
		<view class="home-main" style="height: 50upx;">
			<!-- <com-copyright></com-copyright> -->
		</view>
		
		<dialog-couponshareget @loginAct="showLoginCouponShareGet" v-if="showCouponShareGet" @closed="showCouponShareGet = false"></dialog-couponshareget>
	</view>
</template>

<script>
	export default{
		props: ['datasa'],
		data(){
			return {
				isLogin:false,
				showBirthday:false,
				showLogin:false,
				showQrcode:false,
				showCouponShareGet:false,
			}
		},

		created(){
			
			// uni.setNavigationBarTitle({
			//     title: '夜趣小视频'
			// });
			
			// setTimeout(()=>{
			// 	this.showCouponShareGet = true;	
			// },1000);
		},
		methods:{
			//'/pages/client/tuan/detail?id='+value.id
			detail(vid,mid){
				uni.navigateTo({
					url:'/pages/client/tuan/detail?vid='+vid+'&mid='+mid
				})
			},
			showLoginAct(){
				this.showLogin = true;
			},
			showLoginCouponShareGet(){
				this.showLogin = true;
			},
			loginYes(){
				
			}
		}
	}
</script>

<style>
	.home-header{
		height: 304rpx;
		width: 100%;
		position: relative;
		background: #363B4D;
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
	.tuan-product-l{
		width: 320rpx;
		height: 240rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.integral-mall-goods{
		width: 100%;
		height: 280rpx;
		background: #F2F2F2;
		border-radius: 16upx;
	}
	.tuan-product-r{
		width: calc(100% - 320rpx);
	}
	
</style>
