<template>
	<view class="pd16_15">
		<view class="box noshadow pd16_15" v-if="iskq==1">
			<view class="flex alcenter space">
				<view class="ft16 ftw600 cl-main" style="width: 100%;">我的团队</view>
				<button class="ft12" @click="share" open-type="share" style="width: 200upx; background: #ff0000; color: #ffffff; height: 60upx;">点击分享</button>
			</view>
			<view class="bd-line mt16"></view>
			<view class="flex wrap mt24">
				<block v-for="(value,key) in data" :key="key">
				<view class="col5">
					<view class="text-center">
						<image :src="value.avatar?value.avatar:value.url" class="miniapp-icon"></image>
					</view>
					<view class="text-center ft12 cl-info2 mt8">
						{{value.id}}
					</view>
				</view>
				</block>
			</view>
		</view>	
		<view class="box noshadow pd16_15" v-else>
			<view class="ft16 ftw600 cl-main">系统后台没有开通分销</view>
		</view>	
	</view>
</template>

<script>
	export default{
		data(){
			return {
				data:[],
				iskq:0,
				uid:0,
			}
		},
		onLoad(){
			if(uni.getStorageSync("userinfo")){
				 this.uid=uni.getStorageSync("userinfo").id
			}
			if(uni.getStorageSync("config").iskq){
				this.iskq=uni.getStorageSync("config").iskq;
			}
			this.ongrzlTap();
		},
		onShareAppMessage: function(res) {
			  // #ifdef MP-WEIXIN
			var href='/pages/login/login?fxpid='+this.uid
		    console.log(res)
		    let that = this;
		    const obj = {
		      title: "发送给好友",
		      imageUrl: '',
		      path: href,
		      success: function(res) {
		        console.log(res, "转发成功")
		      },
		      fail: function(res) {
		        wx.showToast({
		          title: '发送失败',
		          icon:'none'
		        })
		      }
		    }
		    return obj
			  // #endif
		},
		methods:{
			share(){
					  // #ifdef H5
					  var href='/pages/login/reg?uid='+this.uid
					  uni.setClipboardData({
					  	data: this.configs.webUrl+'/h5/#'+href,
					  	complete() {
					  		uni.showToast({
					  			title: "分享连接已复制到剪贴板"
					  		})
					  	}
					  })
					  // #endif
					  // #ifndef H5
					  var href='/pages/login/reg?uid='+this.uid
					  uni.share({
					  	provider: "weixin",
					  	scene: "WXSenceTimeline",
					  	type: 0,
					  	href: href,
					  	title: '分享标题',
					  	summary: '分享总结',
					  	imageUrl: '',
					  	success: function(res) {
					  		console.log("success:" + JSON.stringify(res));
					  	},
					  	fail: function(err) {
					  		console.log("fail:" + JSON.stringify(err));
					  	}
					  })
					  // #endif
			},
			async ongrzlTap(){
				let data = {};
				data.token = uni.getStorageSync("userinfo").token;

				uni.request({
					url: this.configs.webUrl+'/api/user/mytuandui',
					data: data,
					success: res =>{
						if(res.data.code==1){
							this.data=res.data.data
		

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
	.miniapp-icon{
		width: 80rpx;
		height: 80rpx;
		border-radius: 40rpx;
		background: #F2F2F2;
	}
	.col5{
		margin-bottom: 20px;
	}
</style>