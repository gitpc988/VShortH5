<template>
	<view>
		<!-- 标题栏和状态栏占位符 -->
		<!-- <view class="titleNview-placing"> </view> -->
		<sub-tabvs class="" :tabs="typetab" @change="changeIndex" :selectIndex="selecttype" :scrollTop="scrollTop"></sub-tabvs>
		<view v-if="selecttype==1">
			<home-jqjs :datasa="listjq"></home-jqjs>
		</view>
		<view v-if="selecttype==2" class="flex space pd10_15" style="display:flex; flex-wrap:wrap;">
			<block v-for="(value,key) in listbz" :key="key">
			<view style="width: 49%; margin-bottom: 20upx;">
				<image style="height: 500upx; width: 100%; border-radius: 18upx;" mode="aspectFill" :src="value.img"></image>
			</view>
			</block>
		</view>
		<view v-if="selecttype==0">
			<view class="home-header pd0_15 mt10" :style="'background: '+mbgColor+';'">
					<view class="home-mendian flex alcenter space plr15 cl-w9" style="display: none;">
						<navigator style="width: calc(100% - 80rpx);" url="/pages/client/tuan/ss">
						<view  class="flex alcenter" style="width: calc(100% - 80rpx);">
							<text class="iconfont  iconsousuo ft14 mr10"></text>
							<text class="text-over ft14" style="width: calc(100% - 60rpx);">请输入搜索内容</text>
						</view>
						</navigator>
						<!-- <text @click="saoma()" class="iconfont iconicon_saoma ft24"></text> -->
					</view>
				
				<view class="">
					<home-banner :banners="banners"></home-banner>
				</view>
			</view>
			<view class="integral-mall-main plr15" style="margin-top: 30upx;">	
				<view class="integal-mall-menu flex pt10 pb10">
					<view class="col2 text-center" @click="linkTo" data-link="/pages/client/tuan/zjjl">
						<view>
							<image style="width: 100rpx; height: 100rpx;" :src="statics.zhuico[0]"></image>
						</view>
						<view class="ft14 ftw600">追剧</view>
					</view>
					<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss">
						<view>
							<image style="width: 100rpx; height: 100rpx;" :src="statics.zhuico[6]"></image>
						</view>
						<view class="ft14 ftw600">最新</view>
					</view>
					<!-- /pages/client/integral/role -->
					<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss?selectIndex=3">
						<view>
							<image style="width: 100rpx; height: 100rpx;" :src="statics.zhuico[2]"></image>
						</view>
						<view class="ft14 ftw600">排行</view>
					</view>
					<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss?selectIndex=2">
						<view>
							<image style="width: 100rpx; height: 100rpx;" :src="statics.zhuico[5]"></image>
						</view>
						<view class="ft14 ftw600">免费</view>
					</view>
				</view>
				
				<view class="mt24" v-if="hotdata[1]">
					<view class="flex alcenter space">
						<view class="flex alcenter">
							<image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[0]"></image>
							<text class="ft16 ftw600 cl-main ml15">本周热门</text>
						</view>
						<!-- <navigator url="/pages/client/tuan/ss?selectIndex=1"> -->
						<view @click="huanyihuan()" class="ft14 cl-notice">换一换</view>
						<!-- </navigator> -->
					</view>
					
					<view class="mt16 flex space">
						<block v-for="(value,key) in hotdata" :key="key" v-if="key>=hyh && key<hyh+3">
						<view class="box pb10" style="width:31%; position: relative; border-radius:20rpx;" @click="detail(value.id,0)">
							<!-- <view class="btn-mini" style="position: absolute; top: 20upx; right: 20upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx; z-index: 1;" :style="getBtnStyle">{{value.lxname}}</view> -->
							<image class="integral-mall-goods" mode="aspectFill" :src="value.img"></image>
							<view class="mt8 plr10 ft14 ftw400 text-center text-over cl-main">{{value.name}}</view>
						</view>
						</block>	
				
					</view>
					
				</view>	
				<view class="mt24" v-if="givedata[3]">
					<view class="flex alcenter space">
						<view class="flex alcenter">
							<image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[2]"></image>
							<text class="ft16 ftw600 cl-main ml15">排行榜</text>
						</view>
						<!-- <navigator url="/pages/client/tuan/ss?selectIndex=3"> -->
						<view class="ft14 cl-notice">每周热剧TOP3</view>
						<!-- </navigator> -->
					</view>
					<view class="mt16 flex space">
						<block v-for="(value,key) in givedata" :key="key" v-if="key<3">
						<view class="integral-mall-coupon" style="width: 31%;" @click="detail(value.id,0)">
							<view class="top">
								<view class="flex center">
									<view class="coupon-value" >
										<!-- <view class="btn-mini" style="position: absolute; top: 10upx; right: 10upx; border-radius: 10upx;font-size: 18upx;width: 60upx; height: 36upx; z-index: 1;" :style="getBtnStyle">{{value.lxname}}</view> -->
										<image class="integral-tuan-l" style="height: 280upx;" mode="aspectFill" :src="value.img"></image>
										<!--<view class="num cl-orange">
											<text class="ft24 ftw600">¥</text>
											<text class="ft24 ftw600 ml4">20</text>
										</view> -->
										<view class="ft14 plr10 cl-main ftw400 text-center text-over mt10">{{value.name}}</view>
									</view>
								</view>
								
							</view>
							<view class="y-l"></view>
							<view class="y-r"></view>
						</view>
						</block>				
					</view>		
				</view>
			</view>	
			<home-default :datasa="datasa"></home-default>
			<!-- <com-footer model="index"></com-footer> -->
		</view>
	</view>
</template>
<script>
	export default{
		data(){
			return {
				hyh:0,
				navLock:false,
				datasa:[],
				listjq:[],
				listbz:[],
				showdyxx:true,
				dataconfig:[],
				banners:[],
				datainfo:[],
				dataindex:[],
				hotdata:[],
				givedata:[],
				
				typetab:[
					{name:'推荐短剧',id:1},
					{name:'剧情介绍',id:2},
					{name:'剧情壁纸',id:3},
				],
				selecttype:0,
				scrollTop:0,
				type:1,
				
				mbgColor:this.$mbgColor,
			}
		},
		computed:{
			
		},
		onPageScroll(e){
			if(e.scrollTop > 44){
				if(this.navLock == false){
					this.navLock = true;
					uni.setNavigationBarColor({
						frontColor:"#000000",
						backgroundColor:"#FFFFFF",
						complete:()=>{
							this.navLock = false;
						}
					});
				}
			}else{
				if(this.navLock == false){
					this.navLock = true;
					let this_=this
					uni.setNavigationBarColor({
						frontColor:"#000000",
						backgroundColor:this_.mbgColor,
						complete:()=>{
							this.navLock = false;
						}
					});
				}
			}
		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		onLoad(e){
			// uni.setStorage({//缓存配置信息
			// 	key: 'webUrl',  
			// 	data: this.configs.webUrl
			// })
			//this.getList();
		},
		onShow() {
			this.getList();
			// this.getList(1);
			// this.getList(2);
			// this.getList(3);
			let this_=this
		},
		methods:{
			changeIndex(index){
				if(this.typetab[index].id){
					this.type=this.typetab[index].id
					//this.getList(0)
				}
				console.log(this.type)
				this.selecttype = index;
			},
			huanyihuan(){
				if(this.hyh<12){
					this.hyh=this.hyh+3
				}
				if(this.hyh>=12){
					this.hyh=0
				}
			},
			getList() {
				let this_=this
				let data = {};
				uni.request({
					url: this.configs.webUrl+'/api/video/indexdata',
					data: data,
					success: data => {
						//console.log(data.data)
						uni.setStorage({//缓存配置信息
							key: 'config',  
							data: data.data.config
						})
						this.banners=data.data.config.banner
						if(data.data.config.name){
							uni.setNavigationBarTitle({
							    title: data.data.config.name
							});
						}
						if(data.data.config.mbgColor){
							this.mbgColor=data.data.config.mbgColor
							uni.setNavigationBarColor({
								frontColor:"#000000",
								backgroundColor:this_.mbgColor,
								complete:()=>{
									this.navLock = false;
								}
							});
						}
						this.listbz=data.data.listbz
						this.listjq=data.data.listjq
						this.datasa=data.data.new
						this.hotdata=data.data.hotdata
						this.givedata=data.data.givedata
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
		/* height: 300rpx; */
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
</style>