<template>
	<view>
		<view class="form">
			<label class="user-name">
				用户名:
			</label>
			<input type="text" placeholder="请输入用户名" @input="namesd" :value="namess">
		</view>
		<view class="form">
			<label class="password">
				密码:
			</label>
			<input type="password" placeholder="请输入密码" @input="passwords" :value="passwordss">
		</view>
		<view class="form">
			<label class="password">
				邮箱:
			</label>
			<input type="email" placeholder="请输入邮箱" @input="mails" :value="mailss">
		</view>
		<view class="form">
			<label class="password">
				手机号:
			</label>
			<input type="tel" placeholder="请输入手机号" @input="tels" :value="telss">
		</view>
		<button @click.prevent="handlefinish">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				namess: "",
				passwordss: "",
				mailss: "",
				telss: "",

				code: "",
				SessionKey: '',
				encryptedData: "",
				iv: "",
				OpenId: '',
				nickName: null,
				gender: 0,
				avatarUrl: null,
				isCanUse: uni.getStorageSync('isCanUse') //默认为true  记录当前用户是否是第一次授权使用的

			}
		},
		methods: {
			namesd(e) {
				this.namess = e.detail.value
			},
			passwords(e) {
				this.passwordss = e.detail.value
			},
			mails(e) {
				this.mailss = e.detail.value
			},
			tels(e) {
				this.telss = e.detail.value
			},
			handlefinish(e) {
				let regEmail = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
				let reg = /^[1]([3-9])[0-9]{9}$/
				if (uni.getStorageSync('name') === this.namess) {
					uni.showToast({
						title: '用户名已存在',
						icon: 'none',
						duration: 2000
					});
				} else if (this.namess === "") {
					uni.showToast({
						title: '用户名不能为空',
						icon: 'none',
						duration: 2000
					});
				} else if (this.passwordss === "") {
					uni.showToast({
						title: '密码不能为空',
						icon: 'none',
						duration: 2000
					});
				} else if (this.mailss === "") {
					uni.showToast({
						title: '邮箱不能为空',
						icon: 'none',
						duration: 2000
					});
				} else if (regEmail.test(this.mailss) == false) {
					uni.showToast({
						title: '邮箱验证失败',
						icon: 'none',
						duration: 2000
					});
					return;
				} else if (this.telss === "") {
					uni.showToast({
						title: '手机号不能为空',
						icon: 'none',
						duration: 2000
					});
				} else if (reg.test(this.telss) == false) {
					uni.showToast({
						title: '手机号验证失败',
						icon: 'none',
						duration: 2000
					});

				} else {
					//将新用户信息存储到localStorage
					uni.setStorageSync('name', this.namess)
					uni.setStorageSync('password', this.passwordss)
					uni.setStorageSync('mail', this.mailss)
					uni.setStorageSync('tel', this.telss)
					uni.setStorageSync('s', "false")
					uni.showToast({
						title: '注册成功',
						icon: 'none',
						duration: 2000
					});
					uni.switchTab({
						url: "/pages/user/user"
					})
				}
			},

		}


		// onLoad() {

		// 	},


	}
</script>

<style lang="scss" scoped>
	.form {
		display: flex;
		justify-content: space-around;
		align-items: center;
		margin-left: 27px;
		margin-bottom: 15px;
		margin-top: 20px;

		input {
			border: 1px solid #fc0;
			width: 200px;
			height: 30px;
			padding-top: 5px;
			border-radius: 8px;
			margin-left: 4px;
		}
	}
</style>
