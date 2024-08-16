<template>
    <div class="page" :style="{ height: height }">
		<swiper
			class="swiper"
			:vertical="true"
			
			@change="changeCurrent"
			@animationfinish="animationFinish"
			:current="index">
			<swiper-item v-for="(item, idx) in videoList" :key="idx" @click="oncfds()" class="swiper-item">
					<!-- 视频渲染数预加载数影响性能 -->
					<div v-if="Math.abs(index-idx)<=1" class="video-box">
						<block v-if="item.src">
							<chunlei-video class="video" :src="item.src" :controls="controls"  :height="height" :width="width"
								:play="item.flag" v-if="Math.abs(index-idx)<=1" :gDuration="item.duration"
								:initialTime="item.initialTime" @pause="pauseVideo" :objectFit="item.objectFit"
								@playEnd="playEnd" @timeupdate="timeupdate"  @screenChange="screenChange" @controlstoggle="controlstoggle" :danmuList="danmuList"
							>
							</chunlei-video>
							
							<cover-view class="cover-view-left" v-if="cdsfhhh">
								<cover-view class="left-view">
									<!-- #ifdef APP-PLUS-NVUE -->
									<text class="left-text">《{{item.vname}}》</text>
									<!-- #endif -->
									<!-- #ifndef APP-PLUS-NVUE -->
									<cover-view class="left-text">《{{item.vname}}》</cover-view>
									<!-- #endif -->
								</cover-view>
								<cover-view class="left-view">
									<!-- #ifdef APP-PLUS-NVUE -->
									<text class="left-text">{{item.content}}</text>
									<!-- #endif -->
									<!-- #ifndef APP-PLUS-NVUE -->
									<cover-view class="left-text">{{item.content}}</cover-view>
									<!-- #endif -->
								</cover-view>
								<cover-view @click.stop="sellxj()" class="left-view">
									<!-- #ifdef APP-PLUS-NVUE -->
									<text class="left-text">🎬 {{item.name}}</text>
									<!-- #endif -->
									<!-- #ifndef APP-PLUS-NVUE -->
									<cover-view class="left-text">🎬 {{item.name}}</cover-view>
									<!-- #endif -->
								</cover-view>
							</cover-view>
							<cover-view class="cover-view-right" v-if="cdsfhhh">
								<!-- <cover-image :src="item.avater" class="avater img" @click.stop="tapAvater"></cover-image> -->
								<!-- #ifdef APP-PLUS-NVUE -->
								<!-- <text class="right-text-avater">+</text>
								<text class="right-text"></text> -->
								<!-- #endif -->
								<!-- #ifndef APP-PLUS-NVUE -->
								<!-- <cover-view class="right-text-avater">+</cover-view> -->
								<!-- <cover-view class="right-text"></cover-view> -->
								<!-- #endif -->
								
								<cover-image :src="item.check?'/static/aixinRed.png':'/static/aixin.png'" class="img" @click.stop="tapLove(item.pid,item.mid)"></cover-image>
								
								<!-- #ifdef APP-PLUS-NVUE -->
								<text class="right-text" @click.stop="tapLove(item.pid,item.mid)">{{item.like}}</text>
								<!-- #endif -->
								<!-- #ifndef APP-PLUS-NVUE -->
								<cover-view class="right-text" @click.stop="tapLove(item.pid,item.mid)">{{item.like}}</cover-view>
								<!-- #endif -->
								
								<cover-image src="/static/share-fill.png" class="img" @click.stop="tapShare(item.pid,item.mid)"></cover-image>
								<!-- #ifdef APP-PLUS-NVUE -->
								<text @click.stop="tapShare(item.pid,item.mid)" class="right-text">分享</text>
								<!-- #endif -->
								<!-- #ifndef APP-PLUS-NVUE -->
								<cover-view @click.stop="tapShare(item.pid,item.mid)" class="right-text">分享</cover-view>
								<!-- #endif -->
								
								<cover-image :src="item.iszj?'/static/scRed.png':'/static/sc.png'" class="img" @click.stop="tapMsg(item.pid,item.mid)"></cover-image>
								<!-- <cover-image src="/static/xiaoxi.png" class="img" @click.stop="tapMsg(item)"></cover-image> -->
								<!-- #ifdef APP-PLUS-NVUE -->
								<text @click.stop="tapMsg(item.pid,item.mid)" class="right-text">{{item.iszj?'已追':'追剧'}}</text>
								<!-- #endif -->
								<!-- #ifndef APP-PLUS-NVUE -->
								<cover-view @click.stop="tapMsg(item.pid,item.mid)" class="right-text">{{item.iszj?'已追':'追剧'}}</cover-view>
								<!-- #endif -->
							</cover-view>
						</block>
						<div :style="{ height: height }" style="text-align: center; justify-content: center; align-items: center;" class="video-box"  v-else>
							<text style="color: #FFFFFF; font-size: 12px;">没有数据</text>
							<text style="color: #FFFFFF; font-size: 10px;">-或者数据加载失败-</text>
						</div>
					</div>

			</swiper-item>
		</swiper>
		<view @click.stop="displayxj"  v-if="sharesa" :class="sharesa?'xunji':''"  :style="{ height: height }">
			<cover-view v-if="sharesa" class="cover-view-lefts" :class="sharesa?'hidelefts':'hidelefts'">
				<cover-view class="left-view" style="color: #333333; height: 80rpx; line-height: 80rpx; font-weight: bold; border-bottom: 1px solid #ddd;">
					选集 | {{videodata.wanji}}
					<cover-view @click.stop="displayxj" style="float: right; text-align: right; width: 80rpx;height: 80rpx; line-height: 80rpx;">关闭</cover-view>
				</cover-view>	
				<cover-view class="left-view" style="max-height: 540rpx; min-height: 400rpx; overflow-y: scroll;">
					<block v-for="(item, keyfj) in videoList" :key="keyfj" >
					<cover-view @click.stop="xunjiup(keyfj)" class="left-texta" :class="item.pays==1?'left-texta1':'left-texta'">
					{{item.fjname}}
						<view v-if="item.pays==1" style="position: absolute; top: 5rpx; text-align: center;width: 100%;">
							<cover-image style="width: 60rpx; height: 60rpx;" mode="aspectFit" src="/static/shilu-login/2.png"></cover-image>
						</view>
					</cover-view>
					</block>
				</cover-view>	
			</cover-view>
		</view>
    </div>
</template>
<script>
	import chunleiVideo from '@/components/chunlei-video/chunlei-video'
    export default { 
		components:{
			chunleiVideo
		},
        data() {
            return {
				vid: 0,
				mid: 0,
				videodata:[],
				sharesa: false,
				currentTime:0,
				tuday:'',
				timer: null,
				sysheight:0,
				playCount:1,//剩余多少视频加载视频列表
				videoList:[],
				height:'667px',
				index:0,
				width:'',
				oldIndex:0,
				urls:'https://xjc.demo.hongcd.com',
				page:1,
				controls:false,
				cdsfhhh:true,
				issp:true,
				danmuList:[
					// {text: '发条弹幕0',color: '#fff',time: 2,avatar:'../../static/avatar.png'},
					// {text: '发条弹幕1',color: '#fff',time: 3,avatar:'../../static/avatar.png'},
					// {text: '发条弹幕2',color: '#fff',time: 4,avatar:'../../static/avatar.png'},
					// {text: '发条弹幕3',color: '#fff',time: 5,avatar:'../../static/avatar.png'},
				],
			}
        },
		created(){
			//#ifdef APP-PLUS
			plus.screen.lockOrientation("portrait-primary")
			//隐藏subnvue
			uni.getSubNVueById('comment').hide()
			uni.getSubNVueById('input-box').hide()
			//#endif
			this.sysheight = uni.getSystemInfoSync().windowHeight
			this.height = `${this.sysheight}px` 
			let width = uni.getSystemInfoSync().windowWidth 
			this.width = `${width}px` 
			// this.videoList.length = 5
			// this.videoList.fill({src:''})
		},
		async mounted() {
			await this.pushVideoList()
			if(!this.index){
				this.$nextTick(()=>{
					this.videoPlay(this.index) 
				})
			}
		},
		onLoad(e) {
			if(e.vid){
				this.vid=e.vid
			}
			if(e.mid){
				this.mid=e.mid
			}
			// #ifdef MP-WEIXIN
				this.controls=true	
			// #endif
			this.currentTime=0
			this.currentTimenum()
		},
		onUnload() {
			if(this.timer){
				this.clearIntervalnum()
			}
		},
		onHide(){
			//plus.navigator.setFullscreen(false);
			for (let item of this.videoList) {
				item.flag = false
			}
			if(this.timer){
				this.clearIntervalnum()
			}
		},
		onShow() {
			uni.setNavigationBarColor({
				frontColor:"#ffffff",
				backgroundColor:"#000000",
				complete:()=>{
					this.navLock = false;
				}
			});
			//plus.navigator.setFullscreen(true); 
			this.videoPlay(this.index) 
		},
        methods: {
			xunjiup(mid){
				this.sharesa = false;
				this.index=mid
				this.cdsfhhh=true
			},
			catchTouchMove() {
			    return false;
			},
			sellxj() {
				this.sharesa = true;
			},
			displayxj() {
				this.sharesa = false;
			},
			oncfds(){
				console.log(this.cdsfhhh);
				if(this.cdsfhhh==false && this.issp==false){
					this.cdsfhhh=true
				}else if(this.cdsfhhh==true && this.issp==false){
					this.cdsfhhh=false
				}else{
					this.cdsfhhh=false
				}
			},
			screenChange(val){
				console.log(val);
			},
			timeupdate(val){
				console.log(val);
			},
			controlstoggle(val){
				this.issp=val
			},
			playEnd(){
				if(this.sharesa==false){
					this.index=this.index+1
					this.cdsfhhh=true
				}
				//this.videoPlay(this.index+1)
			},
			currentTimenum(){
				let that = this;
				this.timer = setInterval(function() {
					that.currentTime=that.currentTime+1
					//console.log(that.currentTime);
				}, 1000);
			},
			clearIntervalnum(){
				let that=this
				if(this.timer) {
				    clearInterval(this.timer);  
				    this.timer = null;  
				}
				if(that.currentTime>0){
					if(uni.getStorageSync("currentTime")){
						var currentTime=uni.getStorageSync("currentTime")*1
					}else{
						var currentTime=0
					}
					if(this.tuday==uni.getStorageSync("tuday")){
						uni.setStorage({//缓存配置信息
							key: 'currentTime',  
							data: this.currentTime*1+currentTime*1
						})
					}else{
						this.currentTime=0;
						uni.setStorage({//缓存配置信息
							key: 'tuday',  
							data: this.tuday
						})
						uni.setStorage({//缓存配置信息
							key: 'currentTime',  
							data: currentTime*1
						})
					}
					console.log(uni.getStorageSync("currentTime"));
				}
			},
			animationFinish(e){
				//#ifdef APP-PLUS
				this.changeCurrent(e)
				//#endif
			},
			changeCurrent(e){
				this.index = e.detail.current; 
				this.$nextTick(()=>{
					
					for (let item of this.videoList) {
						item.flag = false
					}   
					this.videoList[this.index].flag = true
					
				}) 
			},
			pushVideoList(){
				let this_=this
				let promise = new Promise((resolve,reject)=>{
					uni.request({
						url: this_.urls+'/api/video/videoinfo?page='+this.page+'&vid='+this.vid+'&mid='+this.mid+'&token='+uni.getStorageSync("userinfo").token,
						success: (res) => {
							this.videodata=res.data.videodata
							if(res.data.tuday){
								this.tuday=res.data.tuday	
							}
							this.page=this.page+1
							let videoGroup = []
							if(res.data.result){
								for (let item of res.data.result) {
									if(item.type == 'videoSmallCard'){
										videoGroup.push({
											src:item.videourl,
											content:item.text,
											vname:item.vname,
											name:item.name,
											flag:false,
											check:item.check,
											iszj:item.iszj,
											like:item.give,
											comment:'1045',
											at:item.nickname,
											avater:item.avatar,
											id:item.id,
											mid:item.mid,
											pid:item.pid,
											price:item.price*1,
											pays:item.pays*1,
											initialTime:0,
											fjname:item.fjname,
											duration:900,
											// objectFit:'fill'
										})
									}
								}
							}
							console.log(this.videoGroup);
							let len = this.videoList.filter(item=>item.src).length
							for(let i = len;i<len+videoGroup.length;i++){
								this.$set(this.videoList,i,videoGroup[i-len])
							}
							resolve()
						}
					})
				}) 
				return promise
			},
			detail(vid,mid){
				uni.navigateTo({
					url:'/pages/client/tuan/detail?vid='+vid+'&mid='+mid
				})
			},
			tapLove(vid,mid){
				console.log(vid)
				this.videoList[this.index].check = !this.videoList[this.index].check
				
				this.videoList = [...this.videoList]
				
				let this_=this
				let data = {'vid':vid,'mid':mid};
				data.token = uni.getStorageSync("userinfo").token
				data.uid = uni.getStorageSync("userinfo").id
				uni.request({
					url: this_.urls+'/api/user/tapLove',
					data: data,
					success: data => {
						uni.showToast({
							icon:'none',
							title:data.data.msg
						})
						if(data.data.msg=='取消成功'){
							if(this_.videoList[this_.index].like<=1){
								this_.videoList[this_.index].like=0
							}else{
								this_.videoList[this_.index].like=this_.videoList[this_.index].like-1
							}
						}else{
							this_.videoList[this_.index].like=this_.videoList[this_.index].like+1
						}
					},
					fail: (data, code) => {
					}
				});
			},
			tapAvater(){
				//点击索引为${this.index}的头像
				uni.showToast({
					icon:'none',
					title:`关注功能开发中`
				})
			},
			tapMsg(vid,mid){
				console.log(vid)
				this.videoList[this.index].iszj = !this.videoList[this.index].iszj
				this.videoList = [...this.videoList]
				
				let this_=this
				let data = {'vid':vid,'mid':mid};
				data.token = uni.getStorageSync("userinfo").token
				data.uid = uni.getStorageSync("userinfo").id
				uni.request({
					url: this_.urls+'/api/user/videorecord',
					data: data,
					success: data => {
						uni.showToast({
							icon:'none',
							title:data.data.msg
						})
					},
					fail: (data, code) => {
					}
				});
			
			},
			tapShare(vid,mid){
				console.log(vid)
				uni.showToast({
					icon:'none',
					title:`分享功能开发中`
				})
			},
			videoPlay(index){
				let promise = new Promise((resolve,reject)=>{
					resolve()
				})
				promise.then(res=>{
					this.$set(this.videoList[index],'flag',!this.videoList[index].flag)
				})
			},
			pauseVideo(val){
				if(typeof this.videoList[this.oldIndex].initialTime !='undefined') this.videoList[this.oldIndex].initialTime = val
			},
			clickVideo(){
				this.videoList[this.index].flag = !this.videoList[this.index].flag
			}
        },
		watch:{
			index(newVal,oldVal){
				let len = this.videoList.filter(item=>item.src).length
				//加载视频
				if(len - this.index - 1 <= this.playCount){
					this.pushVideoList()
				}
				this.oldIndex = oldVal
			}
		}
    }
</script>
<style scoped>
	.swiper{
		flex: 1;  
		background-color: #000;
	}
	.swiper-item {
	    flex: 1;
	}
    .video {
		flex: 1;
    }
	.video-box{
		flex: 1;
		width: 750rpx;
	}
	.video-boxf{
		height: 200rpx;
		width: 200rpx;
		justify-content: center;
		align-items: center;
	}
	.cover-view-center{
		position: absolute;
		justify-content: center;
		align-items: center;
		opacity: 0.1;
		z-index: 999;
	}
	.cover-view-left{
		position: absolute;
		margin-left: 20upx;
		width: 700rpx;
		bottom: 80upx;
		z-index: 9998;
		font-size: 16px;
		color: #FFFFFF;
		
		/* #ifndef APP-PLUS */
		white-space: pre-wrap;
		text-overflow:ellipsis;
		overflow:hidden;
		/* #endif */
	}

	.cover-view-lefts{
		/* max-height: 700rpx; */
		border-top-left-radius:40rpx; 
		border-top-right-radius:40rpx;
		position: absolute;
		width: 700rpx;
		bottom: 0px;
		padding: 10rpx 25rpx 25rpx 25rpx;
		z-index: 9999;
		font-size: 16px;
		color: #999;
		background: #FFFFFF;
		overflow:hidden;
	}
	.hidelefts {animation:hide 0.5s linear both;}  
	@keyframes hidelefts {
		0% {opacity:1;}
		100% {opacity:0;}
	}
	.left-view{
		margin-bottom: 20upx;
	}
	.left-text{
		font-size: 16px;
		color: #FFFFFF;
	}
	.left-texta{
		font-size: 14px;
		color: #fff;
		width: 24%;
		padding: 20rpx 0px;
		margin-left: 1%;
		text-align: center;
		float: left;
		border-radius: 10rpx;
		background: #DD524D;
		margin-bottom: 20rpx;
		position: relative;
	}
	.left-texta1{
		font-size: 14px;
		color: #ffff;
		width: 24%;
		padding: 20rpx 0px;
		margin-left: 1%;
		text-align: center;
		float: left;
		border-radius: 10rpx;
		background: #dddddd;
		margin-bottom: 20rpx;
		position: relative;
	}
	.avater{
		border-radius: 50upx;
		border-color: #fff;
		border-style: solid;
		border-width: 2px;
	}
	
	.cover-view-right{
		position: absolute;
		top: 40%;
		right: 20upx;
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		flex-direction: column;
		/* #endif */
		z-index: 9998;
	}
	
	.right-text-avater{
		position: absolute;
		font-size: 14px;
		top: 80upx;
		left: 30upx;
		height: 40upx;
		width: 40upx;
		background-color: #DD524D;
		color: #FFFFFF;
		border-radius: 50%;
		text-align: center;
		line-height: 40upx;
		z-index: 999;
	}
	.avater-icon{
		height: 40upx;
		width: 40upx;
		
		color: #fff;
		background-color: #DD524D;
		border-radius: 50%;
		position: absolute;
		left: 30upx;
		top:-20upx;
	}
	
	.right-text{
		text-align: center;
		font-size: 14px;
		color: #FFFFFF;
		margin-bottom: 10upx;
		height: 20px;
	}
	.img{
		height: 100upx;
		width: 100upx;
		opacity: 0.9;
	}
	.page{
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		flex-direction: column;
		/* #endif */
		flex: 1; 
	}
	.xunji{
		width:100%;position:fixed;top:0;left:0;right:0;bottom:0;
	}
</style>
