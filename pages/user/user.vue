<template>
	<view>
		<view class="box" v-if="show">
			<view class="top">
				<view class="top-img">
					<image src="../../static/img/70_18691db0e4af30a5d2ba79d4db14ea7e.jpeg"></image>
				</view>
				<view class="top-r">
					<view class="user">
						杰米16686
					</view>
					<view class="biaoqian">
						正在通往美食达人的路上...
					</view>
				</view>
			</view>
			<view class="back">

			</view>
			<view class="zhongjian">
				<view class="coll">
					<image src="../../static/img/icon_comment_like_2.png"></image>
					<span>我的收藏</span>
				</view>
				<view class="fa">
					<image src="../../static/img/icon_course_live_comment.png"></image>
					<span>我的发布</span>
				</view>
			</view>
			<view class="back">

			</view>
		</view>

		<view class="input-box" v-if="!show">
			<view class="user">
				<view class="user-name">
					用户名:
				</view>
				<input type="text" name="username" placeholder="请输入用户名" @input="userNames" :value="userName">
			</view>
			<view class="password-box">
				<view class="password">
					密 码:
				</view>
				<input type="password" name="password" placeholder="请输入用户名" @input="pass" :value="passwords">
			</view>
		</view>


		<view class="l-g" v-if="!show">
			<view class="login" @click="handlelogin">
				登录
			</view>
			<view class="zhuce" @click="gister()">
				注册
			</view>
		</view>
		<view class="zhuxiao" v-if="show" @click="isShows">
			注销登录
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
				userName: '',
				passwords: '',
				st: 'false',
				loginZhuang: ''
			}
		},
		methods: {
			gister() {
				uni.navigateTo({
					url: '/pages/login/login'
				})
			},
			userNames(e) {
				this.userName = e.detail.value
			},
			pass(e) {
				this.passwords = e.detail.value
			},
			handlelogin: function() {
				if (
					this.userName === uni.getStorageSync('name') &&
					this.passwords === uni.getStorageSync('password')
				) {
					this.show = !this.show
					this.userName = ''
					this.passwords = ''
					this.st = 'true'
					uni.setStorageSync('s', this.st)
					uni.showToast({
						title: '登录成功',
						icon: 'none',
						duration: 1000
					});

				} else if (this.userName === "") {
					//名字为空
					uni.showToast({
						title: '用户名不为空',
						icon: 'none',
						duration: 1000
					});
				} else if (this.passwords === "") {
					//密码为空
					uni.showToast({
						title: '密码不为空',
						icon: 'none',
						duration: 1000
					});
				} else {
					uni.showToast({
						title: '账号不存在，请注册后登录',
						icon: 'none',
						duration: 1000
					});
				}
			},
			isShows() {
				this.show = !this.show
				this.st = 'false'
				uni.setStorageSync('s', this.st)
				uni.showToast({
					title: '注销成功',
					icon: 'none',
					duration: 1000
				});
			},
			loginOrno() {
				let login = uni.getStorageSync('s')
				this.loginZhuang = login
				if (this.loginZhuang == "true") {
					this.show = !this.show
				} else {
					this.show = this.show
				}
			},

		},
		mounted() {
			this.loginOrno()
		},
	}
</script>

<style lang="scss" scoped>
	.top {
		display: flex;
		width: 96vw;
		margin: 20px auto;

		.top-img {
			width: 100px;
			height: 100px;
			border-radius: 5px;
			overflow: hidden;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.top-r {
			margin-left: 7px;

			.user {
				font-weight: bold;
				font-size: 20px;
				margin-bottom: 25px;
				margin-top: 10px;
			}

			.biaoqian {
				color: #CDE;
				font-size: 15px;
			}
		}
	}

	.back {
		width: 100vw;
		height: 10px;
		background: #ccc;
	}

	.coll {
		display: flex;
		align-items: center;
		margin-left: 5px;
		width: 100vw;
		height: 40px;
		line-height: 40px;
		margin-bottom: 5px;
		border-bottom: 1px solid #ccc;

		image {
			width: 20px;
			height: 20px;
		}

		span {
			margin-left: 5px;
		}
	}

	.fa {
		display: flex;
		align-items: center;
		margin-left: 5px;
		width: 100vw;
		height: 40px;
		line-height: 40px;
		margin-bottom: 15px;
		border-bottom: 1px solid #ccc;

		image {
			width: 20px;
			height: 20px;
		}

		span {
			margin-left: 5px;
		}
	}

	.login {
		width: 140px;
		height: 35px;
		line-height: 35px;
		background-color: #26db1687;
		text-align: center;
		margin: 15px auto;
	}

	.zhuce {
		width: 140px;
		height: 35px;
		line-height: 35px;
		background-color: #80808054;
		text-align: center;
		margin: 15px auto;
	}

	.zhuxiao {
		width: 140px;
		height: 35px;
		line-height: 35px;
		background-color: #CED;
		text-align: center;
		margin: 15px auto;
	}

	.user {
		display: flex;
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

	.password-box {
		display: flex;
		align-items: center;
		margin-left: 35px;
		margin-bottom: 15px;
		margin-top: 20px;

		input {
			border: 1px solid #fc0;
			width: 200px;
			height: 30px;
			padding-top: 5px;
			border-radius: 8px;
		}
	}
</style>
