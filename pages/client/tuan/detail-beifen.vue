<template>
	<view>
		<view class="tuan-detail-header">
			<video  id="myVideo" :poster="info.img?info.img:''" :show-center-play-btn="playbtn" :autoplay="autoplay"  style="width: 100%;" @timeupdate="timeupdate"  :src="info.videourl"></video>
		</view>
		<view class="tuan-detail-tit pd20_15">
			<view class="ft18 cl-main ftw600">{{info.name}}</view>
			<view class="flex alcenter space mt12">
				<view class="flex alcenter">
					<text class="ft14 cl-orange">会员价：</text>
					<text class="ft16 cl-orange ftw600">¥{{info.vipprice}}</text>
					<text class="ml10 ft12 cl-notice">普通价：</text>
					<text class="ft12 cl-notice text-line">¥{{info.price}}</text>
				</view>
				<view class="cl-notice ft12">{{info.updatetime}}</view>
			</view>
		</view>
		<view v-if="infolength>1" class="tuan-detail-tit pd16_15 flex space" style="display:flex; flex-wrap:wrap;">
			<view class="ft16 ftw600">选集</view>
			<view @click="opxj()" class="ft14" style="color: #999;">共{{infolength}}集</view>
		</view>
		<view v-if="infolength>1" class="row pl15 pr15" style=" background: #FFFFFF;">
			<scroll-view  :scroll-with-animation="true" :scroll-x="true" class="tab-nav-plus-scroll">
				<view v-for="(value,index) in info.video" :key="index" :data-index="index" @click="op(value.id)" class="item">
					<view class="tit" :class="info.pid==value.id ? 'on' : ''" :style="{color: info.pid==value.id ? '#ffffff': '#353535'}">{{value.name}}</view>
					<!-- <view class="bd" :style="{background:mid == item.k ? tempColor: 'transparent'}"></view> -->
				</view>
			</scroll-view>
		</view>
		<view class="mt24" style="margin:30upx 30upx 0 30upx;box-shadow: 0px 4px 20px 0px rgba(0,0,0,0.04);">
			<home-banner :banners="banners"></home-banner>
		</view>
		<home-default :datasa="datasa"></home-default>
		<view class="form-footer-h">
			<view class="form-footer-h form-footer">
				<view class="form-footer-main pd10_15 flex alcenter space">
						<view class="flex alcenter space" style="width:200upx;margin-right: 20upx;">
							<navigator open-type="reLaunch" url="/pages/client/index">
								<view class="form-footer-item text-center">
									<view class="iconfont iconicon_bottom_home ft22"></view>
									<view class="ft12 mt8">首页</view>
								</view>
							</navigator>
<!-- 							
							<view @click="contactAct" class="form-footer-item text-center ">
								<view class="iconfont iconicon_bottom_call ft22"></view>
								<view class="ft12 mt8">联系</view>
							</view>-->
							<button v-if="info.isplay==1" @click="xiaz(info.videourl)" class="form-footer-item text-center ">
								<view class="iconfont iconicon_arrowdown ft22"></view>
								<view class="ft12 mt8">下载</view>
							</button> 
							<button v-if="info.isplay==0" @click="vipcard" class="form-footer-item text-center">
								<view class="iconfont icontabbar01 ft22"></view>
								<view class="ft12 mt8">VIP</view>
							</button>
						</view>
						<button v-if="info.isplay==0" @click="buyAct(mid)" class="btn-big" style="width: 50%" :style="getBtnStyle">¥{{priced*1}}单集</button>
						<button v-if="info.isplay==0" @click="buyAct(0)" class="btn-big" style="width: 50%" :style="getBtnStyle">¥{{price*1}}全集</button>
						<button v-if="info.isplay==1" class="btn-big" style="width: 100%; color: #ffffff; background: #C0C0C0!important;">{{isplaytext}}</button>
				</view>
			</view>
		</view>
		
		<dialog-login v-if="showLogin" @loginYes="loginYes" @closed="showLogin = false"></dialog-login>
		
	</view>
</template>

<script>
export default {
		data(){
			return {
				infolength:1,
				isLogin:false,
				showLogin:false,
				autoplay:false,
				playbtn:true,
				selectIndex:0,
				tabs:['详情'],
				nextStep:'',
				datasa:[],
				dataconfig:[],
				id:'',
				mid:0,
				info:[],
				dda:'',
				price:'',
				priced:'',
				banners:[],
				isplaytext:'已经购买',
				audioAction: {
				    method: 'pause'
				}
			}
		},
		onLoad(option){
			this.dda=uni.createVideoContext('myVideo')
			if(option.vid){
				this.id=option.vid
			}
			if(option.mid){
				this.mid=option.mid
				
			}
			this.getList();
		},
		onShow() {
			this.$zaudio.stop();
			this.getinfo(this.id);
			if(uni.getStorageSync("userinfo").token){
				this.isLogin = true
				this.showLogin = false
			}
			this.banners=uni.getStorageSync("config").banner
		},
		onHide() {
			this.dda.pause()
		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		methods:{
			timeupdate(event){
					let _this=this;
					let currentTime = event.detail.currentTime 
					if(currentTime>this.info.mrseek && this.info.isplay==0){
						 //dda.exitFullScreen()
						 this.dda.seek(this.info.mrseek)
						 this.dda.pause()
						 this.dda.stop()
						 uni.showModal({
							title: '温馨提示',
							content: '没权限观看当前视频',
							showCancel: false,
							confirmText: "确定",
							success: function (res) {
								if (res.confirm) {
									
								} else if (res.cancel) {
									
								}
							}
						 });
					}
			},
			xiaz(e){
				var that=this
				// #ifdef H5
					window.open(e, "_blank")
				// #endif
				// #ifndef H5
				uni.showLoading({
					title: '加载中'
				});
				uni.downloadFile({
					url: e,
					success: function(res) {
						if(res.tempFilePath){
							that.selseimging(res.tempFilePath)
						}
					},fail(ress) {
						uni.showToast({
							title: '下载失败'
						})
						uni.hideLoading();
					}
				})
				// #endif
			},
			selseimging(tempFilePath){
				var that=this
				uni.saveImageToPhotosAlbum({
					filePath: tempFilePath,
					success: function(red) {
						uni.hideLoading();
						uni.showToast({
							title: '保存相册成功'
						})
					},
					fail(res) {
						uni.hideLoading();
						if (res.errMsg == "saveImageToPhotosAlbum:fail auth deny") {
							uni.showModal({
								title: '您需要授权相册权限',
								success(res) {
									if (res.confirm) {
										uni.openSetting({
											success(res) {
				
											},
											fail(res) {
												console.log(res)
											}
										})
									}
								}
							})
						}
					}
				});
			},
			getinfo(id) {
				let data = {};
				data.id=id
				data.mid=this.mid
				if(uni.getStorageSync("userinfo").token){
					data.token=uni.getStorageSync("userinfo").token
					this.isLogin = true
				}
				uni.request({
					url: this.configs.webUrl+'/api/video/info',
					data: data,
					success: data => {
							if(uni.getStorageSync("userinfo").token){
								if(data.data.isvip>1){
									this.price=data.data.vipprice
									this.priced=data.data.vippriced
								}else{
									this.priced=data.data.priced
									this.price=data.data.price
								}
							}else{
								this.price=data.data.price
								this.priced=data.data.priced
							}
							data.data.pricebuy=this.price
							data.data.pricedbuy=this.priced
							if(data.data.isplayd==1){
								data.data.isplay=1
							}
							this.info=[];
							this.info=data.data
							this.infolength=data.data.video.length
							this.mid=data.data.pid
							if(data.data.isvip>1){
								if(data.data.vipprice*1==0){
									this.isplaytext='VIP全集免费'
								}
								if(data.data.vippriced*1==0){
									this.isplaytext='VIP单集免费'
								}
							}else{
								if(data.data.price*1==0){
									this.isplaytext='全集免费'
								}
								if(data.data.priced*1==0){
									this.isplaytext='单集免费'
								}
							}
							uni.setNavigationBarTitle({
							    title: this.info.name
							});
					},
					fail: (data, code) => {
					}
				});
			},
			getList() {
				let data = {
					//column: 'id,post_id,title,author_name,cover,published_at' //需要的字段名
				};
				var limit=10;
				data.limit=limit
				data.ishot=2
				uni.request({
					url: this.configs.webUrl+'/api/video/lists',
					data: data,
					success: data => {
						
						uni.setStorage({//缓存配置信息
							key: 'config',  
							data: data.data.config
						})
						if (data.data.total>0) {
							this.dataconfig=data.data.config
							this.datasa=data.data.rows
						}
					},
					fail: (data, code) => {
					}
				});
			},
			contactAct(){
				if(this.isLogin == false){
					this.showLogin = true;
					this.nextStep = 'contact';
				}else{
					uni.navigateTo({
						url:'/pages/client/vipcard/adviser'
					})
				}
			},
			vipcard(){
				uni.navigateTo({
					url:'/pages/client/vipcard/index'
				})
			},
			op(mid){
				//ddaas.stop()
				this.mid = mid;
				this.getinfo(this.id)
				let ddaas=uni.createVideoContext('myVideo')
				this.autoplay=true
				this.playbtn=false
			},
			buyAct(mid){
				//console.log(mid);
				//return false; 
				if(this.price*1==0){
					uni.showToast({ title: '免费视频不需要购买',icon:"none" });
					return false; 
				}
				if(this.isLogin == false){
					this.showLogin = true;
					this.nextStep = 'buy';
				}else{
					uni.setStorage({//缓存配置信息
						key: 'buydata',  
						data: this.info
					})
					uni.navigateTo({
						url:'/pages/client/tuan/buy?id='+this.id+'&mid='+mid
					})
				}
			},
			loginYes(){
				if(this.nextStep == 'buy'){
					uni.navigateTo({
						url:'/pages/client/tuan/buy'
					})
				}else{
					uni.navigateTo({
						url:'/pages/client/vipcard/adviser'
					})
				}
			},
			changeIndex(index){
				this.selectIndex = index;
			}
		}
	}
</script>

<style>
	.user-not-vip{
		width: 100%;
		height: 80rpx;
		background: #FDF6EC;
		border-radius: 40rpx;
		border: 2rpx solid #EFC381;
		text-align: center;
		line-height: 76rpx;
		font-size: 24rpx;
		color:#000000;
		font-weight: bold;
	}
	.tuan-detail-header{
		position: relative;
	}
	.tuan-detail-swiper{
		height: 500rpx;
	}
	.tuan-detail-swiper image{
		width: 100%;
		height: 500rpx;
		background: #F2F2F2;
	}
	.tuan-detail-tit{
		width: 100%;
		background: #FFFFFF;
		border-radius: 40rpx 40rpx 0rpx 0rpx;
		position: relative;
		margin-top: 0rpx;
	}
	.tuan-detail-content-tab{
		height: 102rpx;
	}
	.tuan-detail-content{
		min-height: calc(100vh - 600rpx);
		background: #FFFFFF;
	}
	.tuan-product-l{
		width: 320rpx;
		height: 240rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.tuan-product-r{
		width: calc(100% - 320rpx);
	}
	.cl-orange1{
		margin-right: 10upx;
		padding: 15upx;
		border: 1px solid #CCCCCC;
	}
	.mini-btn{
		margin-left: 10upx;
		margin-bottom: 10upx;
	}
	.tab-nav-plus{
		height: 80rpx;
	}
	.tab-nav-plus-main{
		width: 100%;
		height: 80rpx;
		padding: 14rpx 30rpx 0rpx 32rpx;
		/* background: #FFFFFF; */
		/* border-top: 0rpx solid #E4E6ED; */
	}
	.tab-nav-plus-main.fixed{
		/* position: fixed; */
		left: 0;
		z-index: 20;
		background: #f8f8f8;
		box-shadow:0rpx 4rpx 16rpx 0rpx rgba(0,0,0,0.08);
	}
	.tab-nav-plus-scroll{
		width: 100%;
		white-space: nowrap;
		height: 110rpx;
	}
	
	.tab-nav-plus-scroll .item{
		height: 80rpx;
		line-height: 80rpx;
		display: inline-block;
		position: relative;
	
	}
	.itemsa{
		height: 80rpx;
		line-height: 80rpx;
		display: inline-block;
		position: relative;
	
	}
	.tab-nav-plus-scroll .item:last-child{
		margin-right: 0;
	}
	.tab-nav-plus-scroll .item .tit{
		text-align: center;
		font-size: 28rpx;
		font-weight: 300;
		padding: 0 30upx;
		border: solid 1upx #ccc;
		margin-right: 10upx;
		border-radius: 10upx;
	}
	.tab-nav-plus-scroll .item .tit.on{
		font-size: 28rpx;
		font-weight: 600;
		background: #ff0000;
		border: solid 1px #ff0000;
	}
	.tab-nav-plus-main .tab-nav-plus-scroll .item .bd{
		position: absolute;
		z-index: 1;
		left: calc(50% - 18rpx);
		bottom: 0;
		width: 36rpx;
		height: 12rpx;
		border-radius: 6rpx 6rpx 0rpx 0rpx;
	}
	
	.tab-nav-plus-main.fixed  .tab-nav-plus-scroll .item .bd{
		background: #FFFFFF;
	}
</style>