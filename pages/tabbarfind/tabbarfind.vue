<template>
	
	<view>
        <web-view :src="url" v-if="url"/>
		<text style="text-align: center;color: red;" v-else>无效的地址</text>
	</view>
	
</template>

<script>
   import _get from '../../common/_get';
   import _hook from '../../common/_hook';
	
	export default {
		components: {
			
		},
		data() {
			return {
				list:[],
				url: "",
				agent_user_id:0
			}
		},
		onShow(){
			_hook.routeTabBarHook();
		},
		onLoad(option) {
			let _this = this;
			
			_get.getTabbarFindInfo({},function (res) {
			    _this.list = res;
				console.log("发现结果",_this.list)
				_this.url = res.url;
			});
			
			/** 监听新的个人数据 */
			uni.$on('data_user_info', function(data) {
				_this.my_data = data;
				console.log('用户信息',_this.my_data)
				console.log('用户音视频id',_this.my_data.id)
				
				
			});
			
		},
		computed: {
			
		},
		methods: {
			
		},
		onNavigationBarButtonTap(e) {

			console.log(e.index)
			// #ifdef APP-PLUS
				let currentWebview = this.$mp.page.$getAppWebview().children()[0]; //获取当前页面的webview对象
			// #endif
			switch (e.index){
				case 0:
					
					// #ifdef APP-PLUS
						currentWebview.reload();
					// #endif
					
					// #ifdef H5
						location.reload();
					// #endif
					
					break;
				case 1:
					uni.reLaunch({
					    url: '../tabbarfind/tabbarfind'
					});
					break;
				case 2:

					uni.switchTab({
						url: '../push/game',
					});
					
					break;
				case 3:
						uni.navigateBack();
					break;
				default:
					break;
			}
		},
	}
</script>

<style>
	
</style>
