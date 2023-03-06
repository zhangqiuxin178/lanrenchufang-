<template>
	<view>
		<view class="list-search">
			<view class="search-box">
				<view class="input-box" @click="goSearch()">
					<input type="text" :placeholder="keyword" @input="getKs" :value="ks">
					<view class="search-jin">
						<image src="../../static/img/icon_menu_search.png"></image>
					</view>
				</view>
				<view class="search" @click="goSearchList(ks)">搜索</view>
			</view>
		</view>
		<view class="keywords-list">
			<view class="list-item" :class="{active: index == i}" v-for="(k,i) in keywordsList" :key="i"
				@click="getList(k,i)">
				{{k}}
			</view>
		</view>
		<view class="list-box">
			<view class="list" v-for="(item,i) in detailList" :key="i" @click="goDatal(item.r.id)">
				<view class="list-l">
					<image :src="item.r.img"></image>
				</view>
				<view class="list-r">
					<view class="list-title">
						{{item.r.n}}
					</view>
					<view class="user">
						{{item.r.an}}
					</view>
					<view class="list-num">
						{{item.r.recommendation_tag}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				detailList: [],
				secondary_keyword: '',
				keywordsList: [],
				index: 0,
				offset: 0,
				limit: 20
			}
		},
		methods: {
			getList(ksl, index) {
				this.index = index
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url: 'https://apis.netstart.cn/douguo/recipe/search?keyword=' + this.keyword +
						'&order=0&type=0&secondary_keyword=' + this.secondary_keyword + '&offset=' + this.offset +
						'&limit=' + this.limit,
					method: 'GET',
					success: (res) => {
						this.detailList = res.data.result.list
						this.keywordsList = res.data.result.secondary_keywords
						this.secondary_keyword = ksl
						// console.log(res.data);
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
		onLoad(option) { //option为object类型，会序列化上个页面传递的参数
			this.keyword = option.id
			// console.log(this.keyword);
		},
		mounted() {
			this.getList()
		},
		onReachBottom() {
			uni.showLoading({
				title: '加载中'
			});
			this.offset += 10
			this.limit += 10
			uni.request({
				url: 'https://apis.netstart.cn/douguo/recipe/search?keyword=' + this.keyword +
					'&order=0&type=0&secondary_keyword=' + this.secondary_keyword + '&offset=' + this.offset +
					'&limit=' + this.limit,
				method: 'GET',
				success: (res) => {
					this.detailList = [...this.detailList, ...res.data.result.list]
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
				url: 'https://apis.netstart.cn/douguo/recipe/search?keyword=' + this.keyword +
					'&order=0&type=0&secondary_keyword=' + this.secondary_keyword + '&offset=' + this.offset +
					'&limit=' + this.limit,
				method: 'GET',
				success: (res) => {
					this.detailList = [...res.data.result.list, ...this.detailList]
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
				background-color: #ccc;
				height: 22px;
				border-bottom-right-radius: 999px;
				border-top-right-radius: 999px;
			}
		}
	}

	.keywords-list {
		display: flex;
		white-space: nowrap;
		overflow: scroll;

		&::-webkit-scrollbar {
			display: none;
		}

		.list-item {
			padding: 5px;
		}

		.active {
			color: #fc0;
		}
	}

	.list {
		display: flex;
		margin: 15px auto;
		width: 94vw;

		.list-l {
			flex: 0 0 141px;
			width: 147px;
			height: 90px;
			border-radius: 6px;
			overflow: hidden;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.list-r {
			margin-left: 5px;

			.list-title {
				width: 150px;
				margin-bottom: 10px;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis
			}

			.user {
				margin-bottom: 10px;
				font-size: 12px;
				color: #666;
			}

			.list-num {
				font-size: 12px;
				color: #666;
			}
		}
	}
</style>
