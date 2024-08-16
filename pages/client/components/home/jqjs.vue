<template>
	<view>
		<view class="pd16_15" style="margin-bottom: 80upx;">	
			<view class="" style="">
				<block v-for="(value,key) in datasa" :key="key">
				<view @click="detail(value.id,0)" style="" class="flex alcenter mb16 showStyle">
					<image mode="aspectFill" class="tuan-product-l" :src="value.img"></image>
					<!-- <view class="btn-mini" style="position: absolute; top: 40upx; left: 280upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx;" :style="getBtnStyle">{{value.lxname}}</view> -->
					<view class="tuan-product-r pl15">
						<view class="ft16 ftw600 cl-main text-over1">{{value.name}}</view>
						<view class="mt5">
							<text class="ft14 cl-main text-over4" style="line-height: 50upx;">{{value.story}}</text>
						</view>
						<!-- <view class="mt16 flex space alcenter">
							<view class="ft14 cl-notice">{{value.mname}}</view>
							<view class="btn-vip-adviser ml15">继续观看</view>
						</view> -->
					</view>
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
	.showStyle::before{
		content:'';
		position:'absolute';
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		background-color: rgba(31, 33, 41, 0.4);
		z-index:-1;
		backdrop-filter:blur(80px);
	}
	.showStyle{
		position: relative; 
	}
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
		width: 200rpx;
		height: 260rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.tuan-product-r{
		width: calc(100% - 200rpx);
		height:260rpx;
	}
	.btn-vip-adviser{
		width: 200rpx;
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
	.integral-mall-goods{
		width: 100%;
		height: 280rpx;
		background: #F2F2F2;
		border-radius: 16upx;
	}
	
</style>
