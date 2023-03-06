<template>
	<view>
		<view class="list-box">
			<view class="list-search">
				<view class="search-box">
					<view class="input-box" @click="goSearch()">
						<input type="text" placeholder="搜你想吃的">
						<view class="search-jin">
							<image src="../../static/img/icon_menu_search.png"></image>
						</view>
					</view>
					<view class="search" @click="goSearch()">搜索</view>
				</view>
			</view>
			<view class="day">
				- 今日编辑精选 -
			</view>
			<view class="list-item" v-for="(item,i) in homeList" :key="i">
				<view class="list-title">
					{{item.t}}
				</view>
				<view class="user-box">
					<view class="user-name">
						{{item.u.n}}
					</view>
					<view class="right">
						<view class="r-v">
							<view class="r-v-img">
								<image src="../../static/img/design_ic_visibility.png"></image>
							</view>
							<view class="Views">
								{{item.views}}
							</view>
						</view>
						<view class="r-f">
							<view class="r-f-img">
								<image src="../../static/img/icon_friend_unfavorite.png"></image>
							</view>
							<view class="favorites">
								{{item.fc}}
							</view>
						</view>
					</view>
				</view>
				<view class="list-img" @click="goDatal(item.item_id)">
					<image :src="item.img"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				homeList: [],
				offset: 0,
				list: 10,
			}
		},
		methods: {
			getHomeList() {
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url: 'https://apis.netstart.cn/douguo/home/ffeeds/' + this.offset + '/' + this.list,
					method: 'GET',
					success: (res) => {
						this.homeList = res.data.result.rfs
						// console.log(this.homeList);
						setTimeout(function() {
							uni.hideLoading();
						}, 100);
					}
				})
			},
			goDatal(id) {
				uni.navigateTo({
					url: "/pages/home/detail/detail?id=" + id
				})
			},
			goSearch() {
				uni.navigateTo({
					url: "/pages/search/search"
				})
			}
		},
		mounted() {
			this.getHomeList()
		},
		onReachBottom() {
			this.offset += 10
			this.list += 10
			uni.showLoading({
				title: '加载中'
			});
			uni.request({
				url: 'https://apis.netstart.cn/douguo/home/ffeeds/' + this.offset + '/' + this.list,
				method: 'GET',
				success: (res) => {
					this.homeList = [...this.homeList, ...res.data.result.rfs]
					// console.log(this.homeList);
					setTimeout(function() {
						uni.hideLoading();
					}, 100);
				},
				fail: () => {
					uni.showToast({
						title: '网络超时',
						icon: 'none',
						duration: 1000
					});
					setTimeout(function() {
						uni.hideLoading();
					}, 100);
				}
			})

		},
		onPullDownRefresh() {
			this.offset += 1
			this.list += 2
			uni.showLoading({
				title: '加载中'
			});
			uni.request({
				url: 'https://apis.netstart.cn/douguo/home/ffeeds/' + this.offset + '/' + this.list,
				method: 'GET',
				success: (res) => {
					this.homeList = [...res.data.result.rfs, ...this.homeList]
					// console.log(this.homeList);
					setTimeout(function() {
						uni.hideLoading();
					}, 100);
					uni.stopPullDownRefresh()
				},
				fail: () => {
					uni.showToast({
						title: '网络超时',
						icon: 'none',
						duration: 1000
					});
					setTimeout(function() {
						uni.hideLoading();
					}, 100);
				}
			})
		}
	}
</script>

<style lang="scss" scoped>
	.list-search {
		display: flex;
		width: 100vw;
		height: 50px;
		margin: 0 auto;
		margin-bottom: 10px;
		justify-content: center;
		align-items: center;

		.search-box {
			display: flex;
			justify-content: center;
			align-items: center;

			.input-box {
				position: relative;

				input {
					margin: 0 auto;
					padding-left: 20px;
					width: 200px;
					height: 30px;
					line-height: 30px;
					background-color: #66666612;
					border: 1px solid #fc0;
					border-top-left-radius: 999px;
					border-bottom-left-radius: 999px;
				}

				.search-jin {
					position: absolute;
					top: 6px;
					left: 5px;
					width: 15px;
					height: 15px;

					image {
						width: 100%;
						height: 100%;
					}
				}
			}

			.search {
				padding: 5px 15px;
				background-color: #cccccc96;
				height: 22px;
				border-bottom-right-radius: 999px;
				border-top-right-radius: 999px;
			}
		}
	}

	.day {
		margin-bottom: 10px;
		text-align: center;
		font-weight: bold;
		font-size: 18px;
	}

	.list-item {
		width: 94vw;
		margin: 0 auto;
		margin-bottom: 20px;

		.list-title {
			overflow: hidden; //隐藏文字
			text-overflow: ellipsis; //显示 ...
			white-space: nowrap; //不换行
			margin-bottom: 3px;
			font-size: 18px;
			font-weight: bold;
		}

		.user-box {
			display: flex;
			justify-content: space-between;
			margin-bottom: 10px;
			font-size: 12px;
			color: #666;
			align-items: center;

			.right {
				display: flex;

				.r-v {
					display: flex;

					.r-v-img {
						width: 10px;
						height: 10px;

						image {
							width: 100%;
							height: 100%;
						}
					}

					.Views {
						margin-left: 3px;
						margin-right: 10px;
					}
				}

				.r-f {
					display: flex;

					.r-f-img {
						width: 10px;
						height: 10px;

						image {
							width: 100%;
							height: 100%;
						}
					}

					.favorites {
						margin-left: 3px;
					}
				}


			}
		}

		.list-img {
			width: 100%;

			image {
				width: 100%;
			}
		}
	}
</style>
