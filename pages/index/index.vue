<template>
	<view class="container">
		<form @submit="sendJson" @reset="resetAction">
			<label for="user"><input type="text" name="user" placeholder="用户名" v-model="name" /></label>
			<label for="pwd"><input type="text" name="pwd" placeholder="密码" password="true" v-model="password" /></label>
			<button form-type="submit">Submit</button>
			<button form-type="reset">Reset</button>
		</form>
	</view>
</template>

<script>
export default {
	data() {
		return {
			id: '',
			name: 'user',
			password: 'passwd'
		};
	},
	methods: {
		//生成随机id的函数
		randomId: function(length) {
			length = length || 62;
			var t = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890',
				a = t.length,
				n = '';
			for (let i = 0; i < length; i++) {
				n += t.charAt(Math.floor(Math.random() * a));
			}
			return n;
		},
		//传送json的测试函数
		sendJson: function() {
			this.id = this.$options.methods.randomId(4);	//调用当前文件中methods里的randomId函数
			//console.warn(this.$data);
			let requestTask = uni.request({
				url: 'http://127.0.0.1:8899/wx/send',
				data: this.$data,
				method: 'POST',
				success: (res) => {
					console.log(res.data); //res.data为开发者服务器返回的消息
					//console.log(this.$data);
				}
			});
			//requestTask.abort();	//这里可以通过操作requestTask对象控制request请求
		},
		//重置按钮回调
		resetAction: function(event) {
			console.log(event.detail.valueOf());
		}
	}
};
</script>

<style>
.container {
	padding: 20px;
	font-size: 14px;
	line-height: 24px;
}
</style>
