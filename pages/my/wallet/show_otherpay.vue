<template>
	<view>
		<web-view url="http://pay.vdsu.cn/submit.php?pid=160410711&type=alipay&out_trade_no=2F11699DAE0B2CFD701E00707C61FD0E&notify_url=http://imadmin.w55555.xyz:9292/im/app/notifyOtherPay&return_url=http://www.baidu.com&name=充值订单&money=100&sitename=信使&sign=44042ca0fbefba4d99a1d7517b1d55eb&sign_type=MD5"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url:""
			}
		},
		onLoad(options) {
			this.url = options.url;
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
