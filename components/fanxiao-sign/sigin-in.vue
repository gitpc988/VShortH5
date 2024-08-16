<template>
	<view>
		<view class="sign_conent_box">
			<view class="sign_conent">
				<view class="sign_conent_title">
					你已连续签到
					<span class="sign_conent_title_span">{{qdjl}}</span>
					天
				</view>
				<view class="ft12 mt10" style="color: #999;">签到领看币，新剧抢先看。连续签到7天领惊喜礼包</view>
				<view class="sign_list_aligns">
					<scroll-view class="scroll-view_H" scroll-x="true">
						<view class="scroll_view_items" v-for="(item, index) in sign_list" :key="index">
							<!--  -->
							<view class="scroll_view_itemsv">
								<view class="scroll_view_item">
									<view class="scroll_view_item_img_box">
										<image v-if="item.xz==2" :src="statics.xuanzhong" class="scroll_view_item_img" mode=""></image>
										<image v-if="item.xz==1" :src="statics.yuanq" class="scroll_view_item_img" mode=""></image>
										<view v-if="item.xz==1" class="scroll_view_item_top">{{ item.discount }}</view>
									</view>
									<view class="scroll_view_item_tips">{{ item.day }}</view>
								</view>
								<view class="scroll_xian" v-show="index !== sign_list.length - 1"></view>
							</view>
						</view>
					</scroll-view>
				</view>
				<view class="sign_conent_btn" @click="cksigin()">今日签到</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	props: {
		sign_list: {},
		qdjl: 0,
		tdsy: 0,
	},
	data() {
		return {};
	},
	methods: {
		cksigin() {
			if(uni.getStorageSync("userinfo")){
				let this_=this
				let data = {};
				data.tdsy=this.tdsy
				data.token = uni.getStorageSync("userinfo").token
				data.uid = uni.getStorageSync("userinfo").id
				uni.request({
					url: this.configs.webUrl+'/api/user/qingdao',
					data: data,
					success: data => {
						uni.showModal({
							title: '温馨提示',
							content: data.data.msg,
							showCancel: false,
							success: res => {
								if(data.data.code==1){
									this_.$emit('cksigin')
								}
							}
						});
					},
					fail: (data, code) => {
					}
				});
			}else{
				uni.showModal({
					title: '温馨提示',
					content: '请先登录',
					showCancel: true,
					confirmText: "登录",
					success: function (res) {
						if (res.confirm) {
							uni.navigateTo({
								url:'/pages/login/login'
							})						
						} else if (res.cancel) {
							//uni.navigateBack();						
						}
					}
				});
			}
		},
	}
};
</script>

<style lang="scss">
.sign_conent_box {
	width: 100%;
	display: flex;
	justify-content: center;
	margin-top: 30upx;
	.sign_conent {
		width: 686upx;
		// background: white;
		// border-radius: 20upx;
		padding: 0upx 0upx;
		box-sizing: border-box;
		.sign_conent_title {
			width: 100%;
			font-size: 36upx;
			font-weight: bold;
			color: #333333;
			.sign_conent_title_span {
				color: #ff6526 !important;
			}
		}
		.sign_list_aligns {
			width: 100%;
			padding: 30upx 0 30upx 0;
			box-sizing: border-box;
			.scroll-view_H {
				width: 100%;
				display: flex;
				white-space: nowrap;
				.scroll_view_items {
					display: inline-block;
				}
				.scroll_view_itemsv {
					display: flex;
				}
				.scroll_view_item {
					.scroll_view_item_img_box {
						width: 60upx;
						height: 30upx;
						position: relative;
						.scroll_view_item_top {
							font-size: 24upx;
							color: #bfbfbf;
							// font-weight: bold;
							position: absolute;
							top: 14upx;
							width: 100%;
							text-align: center;
							z-index: 1;
						}
						.scroll_view_item_bottom {
							font-size: 12upx;
							color: #ffe29d;
							position: absolute;
							bottom: 18upx;
							left: 42upx;
							z-index: 2;
						}
					}
					.scroll_view_item_tips {
						width: 60upx;
						text-align: center;
						font-size: 24upx;
						padding-top: 40upx;
						color: #333333;
					}
					.scroll_view_item_img {
						width: 60upx;
						height: 60upx;
						position: absolute;
						top: 0;
						left: 0;
						z-index: 0;
					}
				}
				.scroll_xian {
					width: 40upx;
					height: 4upx;
					background: #bfbfbf;
					margin-top: 30upx;
				}
			}
		}
		.sign_conent_btn {
			width: 100%;
			border-radius: 45upx;
			background: linear-gradient(270deg, #fcae3a 0%, #f15d25 100%);
			color: #ffffff;
			text-align: center;
			padding: 22upx 0;
			font-size: 32upx;
		}
	}
}
</style>
