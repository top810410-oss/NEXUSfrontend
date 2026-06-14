<template>
	<view>

		<view>
			<uni-popup ref="popup" type="center">
				<image class="background-img" src="@/static/uni-sign-in/background.png" mode="width">
				</image>
				<view class="content">
					<view class="main">
						<text class="title">{{signTitle}}</text>
						<text class="total">本轮已签到{{days}}天</text>
						<text class="scores">总积分：{{scores}}</text>
					</view>
					<view>
						<view class="days-box">
							<view class="days" v-for="(item,index) in weekdays" :key="index">
								<uni-icons v-if="days>=item" class="icon active" color="#FFFFFF"
									type="checkmarkempty"></uni-icons>
									
								<template v-else>
									<!-- <uni-icons v-if="item<signInRes.n" class="icon active" color="#FFFFFF"
										type="closeempty"></uni-icons> -->
									<uni-icons class="icon" type="checkmarkempty" color="#FFFFFF"></uni-icons>
								</template>
								
								<template>
									<text class="day" :class="{grey:days<item}">第{{item}}天</text>
								</template>

							</view>
						</view>
						<view class="tip-box">
							<text class="tip">签到一次得100积分</text>
							<view class="row">
								<text class="tip">连续签到7天可多得</text>
								<text class="red">688</text>
								<text class="tip">积分</text>
							</view>
						</view>
					</view>
					<uni-icons @click="closeMe" class="close-icon" type="closeempty" size="20" color="#AAAAAA">
					</uni-icons>
				</view>
			</uni-popup>
		</view>


		<view class=" my_padding">
			<!--<navigator url="/pages/my/details"-->
			<!--hover-class="navigator-hover">-->
			<div style="display: flex;" @tap="goPath('/pages/my/details')">
				<view class="uni-media-list-logo phto">
					<image :src="myPhoto" :lazy-load="true" style="border-radius: 10px;" />
				</view>
				<view class="header-msg">
					<view class="header-msg-bottom" style="font-weight: 400;font-size: 24px;color: black">
						{{my_data.nickname}}</view>
					<view class="header-msg-top" style="font-weight: 400;font-size: 20px;">{{my_data.phone}}</view>
				</view>
			</div>
			<image class="header-erw" src="/static/theme/default/my/qrcode.png" :lazy-load="true"
				@tap="goPath('/pages/my/qrcode')" />

		</view>
		<!--</navigator>-->



		<uni-list class="list" style="margin-top: 14px;">
			<uni-list-item v-show="actionSign" title="签到" :showArrow="true" :show-arrow="true" badge-type="error"
				thumb="../../static/theme/default/my/qiandao.png" @click="signIn()" />
			<uni-list-item title="我的零钱" :showArrow="true" :show-arrow="true" badge-type="error"
				thumb="../../static/theme/default/my/lingqian.png" @click="goPath('./wallet/index')" />
			<uni-list-item title="我的收藏" :show-arrow="true" :showArrow="true" thumb="/static/theme/default/my_store.png"
				@click="goPath('store')" />
			<!--<uni-list-item title="插件中心"-->
			<!--:showArrow="true"-->
			<!--:show-arrow="true"-->
			<!--:show-badge="true"-->
			<!--badge-type="error"-->
			<!--:badge-text="show_pay_tips"-->
			<!--thumb="../../static/theme/default/my/chajian.png"-->
			<!--@click="goPath('/pages/my/vendor')"/>-->
			<!-- <uni-list-item title="扫一扫"
			:show-badge="true"
			:showArrow="true"
			badge-type="error"
			thumb="/static/theme/default/push/scan.png"
			@click="goScanCode"/> -->
			<uni-list-item title="联系客服"
                           :showArrow="true"
                           :show-arrow="true"
                           badge-type="error"
                           thumb="../../static/theme/default/kefu.png"
                           @click="goKefu" v-if="showKefu"/>
			<!-- <uni-list-item title="生活圈"
                           :show-badge="true"
                           :showArrow="true"
                           badge-type="error"
                           :badge-text="show_tips"
                           thumb="/static/theme/default/push/circle.png"
                           @click="goPath('../push/circle')"/> -->
			<uni-list-item title="我的动态" :show-badge="true" :showArrow="true" badge-type="error" :badge-text="show_tips"
				thumb="../../static/theme/default/my/dongtai.png"
				@click="goPath('../push/circle_user?user_id='+my_data.id)" />

			<uni-list-item title="收款码" :showArrow="true" thumb="../../static/theme/default/my/qrcode.png"
				@click="goPath('../my/pay/pay')" v-if="0" />
			  <uni-list-item title="帮助中心"
						  :showArrow="true"
						  thumb="../../static/theme/default/help.png"
						  @click="goPath('../article/article')"/>

		</uni-list>


		<uni-list class="list" v-if="0">
			<uni-list-item title="小程序" :show-arrow="true" :showArrow="true"
				thumb="../../static/theme/default/push/program.png" />

			<uni-list-item title="相册" :show-arrow="true" :showArrow="true"
				thumb="../../static/theme/default/my/images.png" @click="goPath()" />
			<uni-list-item title="实名" :show-arrow="true" :showArrow="true"
				thumb="../../static/theme/default/my/real.png" @click="goPath()" />
			<uni-list-item title="表情" :showArrow="true" :show-arrow="true"
				thumb="../../static/theme/default/my/emoji.png" @click="goPath()" />
		</uni-list>

		<uni-list class="list" style="margin-bottom: 20px;margin-top: 14px;">

			<uni-list-item title="设置" :show-arrow="true" :showArrow="true" thumb="../../static/theme/default/my/set.png"
				@click="goPath('../set/index')" />
		</uni-list>

	</view>
</template>

<script>
	import uniList from '../../components/uni-ui/uni-list/uni-list.vue';
	import uniListItem from '../../components/uni-ui/uni-list-item/uni-list-item.vue';
	import _get from '../../common/_get';
	import _hook from '../../common/_hook';
	import _data from '../../common/_data';

	export default {
		components: {
			uniList,
			uniListItem
		},
		data() {
			return {
				signTitle:'今日签到成功',
				actionSign:0,
				total: 0,
				scores: 10,
				days: 0,
				weekdays: [1, 2, 3, 4, 5, 6, 7],
				signInRes: {
					days: [1, 2],
					n: 3
				},
				my_data: {
					id: 0
				},
				show_tips: '',
				show_pay_tips: ''
			}
		},
		onShow() {
			_hook.routeTabBarHook();
			let num = _data.data('no_reader_circle_chat_num'),
				_this = this;
			_this.my_data = _data.data('user_info');

			/** 监听新的个人数据 */
			uni.$on('data_user_info', function(data) {
				_this.my_data = data;
				console.log(_this.my_data)
			});
			/** 监听朋友圈动态提示 */
			// uni.$on('data_circle_tips', function (data) {
			//     _this.show_tips = data;
			// });
			// uni.$on('data_pay_tips',function (data) {
			//     _this.show_pay_tips = data;
			// })
			// if (num) {
			//     this.show_tips = num;
			// } else if (_data.data('no_reader_circle')) {
			//     this.show_tips = '好友动态';
			// } else {
			//     this.show_tips = '';
			// }

			_get.getDbSysConfig({
				'key': 'action_sign'
			}, function(res) {
				console.log("全局签到状态", res);
				if(res.value == 1){
					_this.actionSign = 1
				}else {
					_this.actionSign = 0
				}
			});

		},
		onLoad() {

		},
		onHide() {
			//uni.$off('data_user_info');
			uni.$off('data_circle_tips');
		},
		computed: {
			myPhoto() {
				return _data.staticPhoto() + this.my_data.photo;
			},
			showKefu() {
				return this.my_data.id != 5880
			}
		},
		methods: {
			signIn() {
				let _this = this;
				//	console.log(this.my_data.id)
				_get.signDay({
					'user_id': _this.my_data.id
				}, function(res) {
					if(res.msg == '今日已签到'){
						_this.signTitle = '今日已签到'
					}
					console.log("签到结果", res);
					_this.scores = res.scores;
					_this.days = res.signdays;
				});

				//	console.log(this.scores)
				//普通签到
				this.$refs.popup.open()
			},
			closeMe(e) {
				this.$refs.popup.close()
			},
			goPath(path) {
				console.log(1111)
				if (path) {
					uni.navigateTo({
						url: path,
					});
				}
			},
			goKefu() {
				uni.navigateTo({
					'url': '/pages/chat/message?list_id=' + _data.data('kefu_list_id')
				})
			},
			goScanCode() {
				// uni.navigateTo({
				//   url: '../chat/videoCall'
				// });
				let _this = this;
				uni.scanCode({
					success: function(res) {
						/** 验证必须是一个地址 */
						if (/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\*\+,;=.]+$/.test(
								res.result)) {
							/** 本应用页面 */
							console.log(res.result);
							if (new RegExp(_data.data('http_url')).test(res.result)) {
								if (res.result.match(/\/([a-z]+_[a-z]+)\/(.+)$/) && RegExp.$1 && RegExp.$2) {
									switch (RegExp.$1) {
										case 'chat_add':
											uni.navigateTo({
												url: '/page/details/index?user_id=' + RegExp.$2 +
													'&is_type=3',
											});
											break;
										case 'group_add':
											let option = (RegExp.$2 + '').split('&');
											_this.$httpSend({
												path: '/im/message/addChat',
												data: {
													users: option[1],
													list_id: option[0],
													type: 1,
												},
												success_action: true,
												success(res) {
													let tips = res.err ? res.msg :
														'已经申请加入群聊,请耐心等待群管理审核';
													uni.showModal({
														content: tips,
														showCancel: false,
													});
												}
											});
											break;
										default:
											uni.showModal({
												content: '二维码内容：' + res.result,
												showCancel: false,
											});
											break;
									}
								} else {
									uni.showModal({
										content: '二维码内容：' + res.result,
										showCancel: false,
									});
								}
							} /** 打开新地址 */
							else {
								uni.navigateTo({
									url: '../push/web?url=' + encodeURIComponent(res.result),
								});
							}
						} else {
							uni.showModal({
								content: '二维码内容：' + res.result,
								showCancel: false,
							});
						}
					},
					fail(e) {
						console.log(e);
						return false;
						return;
						uni.showToast({
							title: '扫码错误：' + JSON.stringify(e),
							duration: 2000,
						});
					},
				});
			},
		},
		watch: {},
	}
</script>

<style lang="scss">
	.iconfonts {
		position: absolute;
		right: 160rpx;
		bottom: 0px;
		font-size: 16px;
	}

	.header-msg {
		padding-left: 10px;
	}

	.phto {
		width: 106upx;
		height: 106upx;
		margin-left: 30 upx;
		margin-right: 30 upx;
	}

	.list {
		margin-top: 10upx;
	}

	.qrcode {
		width: 50 upx;
		height: 50 upx;
		margin-right: -20 upx;
	}

	.my_padding {
		padding: 50px 15px 16px 15px;
		display: flex;
		background-color: #fff;
	}

	.uni-media-list-logo {
		padding: 2px;
		border-radius: 10px;
		height: 70px !important;
		width: 70px !important;
		margin-right: 0upx !important;
		margin-left: 0upx !important;
	}

	.my_padding:after {
		font-family: uniicons;
		content: '\e583';
		position: absolute;
		right: 15px;
		top: 15%;
		font-size: 24px;
		color: #bbb;
		-webkit-transform: translateY(-50%);
		transform: translateY(-50%);
	}

	.header-erw {
		position: absolute;
		right: 45px;
		top: 13%;
		font-size: 24px;
		width: 24px;
		height: 24px;
		z-index: 9999;
	}

	.uni-list {
		margin-bottom: 30 upx;
	}

	.uni-list-cell-navigate.uni-navigate-right:after {
		font-size: 20px !important;
	}





	view {
		display: flex;
		box-sizing: border-box;
		flex-direction: column;
	}


	scroll-view {
		-webkit-overflow-scrolling: touch;
	}

	.background-img {
		width: 600rpx;
		height: 600rpx;
	}

	.content {
		position: absolute;
		width: 600rpx;
		height: 600rpx;
		justify-content: space-around;
		align-items: center;
		border-radius: 10px;
		z-index: 5;
	}

	.background-img,
	.content {
		top: 0;
	}

	.main {
		align-items: center;
	}

	.title {
		font-size: 44rpx;
		color: #FFFFFF;
		font-weight: 600;
	}

	.total {
		background-color: #FFFFFF;
		color: #ed5200;
		text-align: center;
		border-radius: 100px;
		font-size: 30rpx;
		margin: 16rpx 0;
		width: 230rpx;
	}

	.scores {
		text-align: center;
		color: #FFFFFF;
	}

	.days-box {
		flex-direction: row;
	}

	.days {
		margin: 12rpx;
		font-size: 19rpx;
		justify-content: center;
		align-items: center;
		color: #f8692c;
	}

	.grey {
		color: #C0C0C0;
	}

	.days-box .icon {
		background-color: #feefeb;
		border-radius: 100px;
		height: 50rpx;
		line-height: 50rpx;
		text-align: center;
		width: 50rpx;
		margin-bottom: 6rpx;
	}

	.days-box .icon.active {
		background-image: linear-gradient(to top, #FF9002, #FF5100);
		background-color: #ffff7f;
	}

	.content,
	.days-box {
		padding: 26rpx;
	}

	.tip-box {
		justify-content: center;
	}

	.tip-box .row {
		flex-direction: row;
		justify-content: center;
	}

	.tip,
	.red {
		text-align: center;
		font-size: 24rpx;
		color: #999999;
		height: 30rpx;
		line-height: 30rpx;
	}

	.red {
		color: #ff0000;
	}

	.close-icon {
		margin-bottom: -180rpx;
		background-color: #FFFFFF;
		border-radius: 100px;
		text-align: center;
		height: 46rpx;
		line-height: 46rpx;
		width: 46rpx;
	}
</style>
