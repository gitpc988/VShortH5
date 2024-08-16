<template>
	<view class="index">

		<!-- <index-header bgColor="bg-index-header" @headerSwitch="headerSwitch"/> -->

		<!-- 视频组件 -->
		<cu-video class="video" :video_list="videoList"/>

	</view>
</template>

<script>
	import indexHeader from '@/components/index-header.vue';
	import cuVideo from '@/components/cu-video.vue';
	export default {
		components: {
			indexHeader,
			cuVideo
		},
		data() {
			return {
				title: 'Hello',
				videoList:[]
			}
		},
		onLoad() {
			this.getVideoList();
		},
		onShow() {
			console.log('界面显示')
		},
		methods: {
			getVideoList(refresh) {
				console.log(refresh)
				uni.request({
					url: 'https://xjc.demo.hongcd.com/api/video/videoRecommend?page='+this.page,
					dataType: 'json',
					success: res => {
						console.log(res.data.result);
						for (let item of res.data.result) {
							this.videoList.push({
								video_id: item.id,
								nickname: item.nickname,
								video_describe: item.name,
								cover_url: item.avatar,
								video_url: item.videourl,
								dianzan: '100',
								pinglun: '90',
								zhuanfa: '80',
								is_dianzan: '70',
								flag: false
							});
						}

					}
				});
			},
			// 顶部切换
			headerSwitch(val){
				// console.log(val);
				this.getVideoList();
			}
			
			
			
			
		}
		
	}
</script>

<style lang="less">
	.index {
		display: flex;
		flex-direction: column;
		/* align-items: center; */
		justify-content: center;
		background-color: #333333;
		height: 100vh;
	}
	.tabbar{
		.action{
			.plus{
				image{
					width: 100upx;
					height: 60upx;
				}
			}
		}
	}
</style>
