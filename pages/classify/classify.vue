<template>
	<view>
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
		<view class="category">
			<view class="l-list">
				<view class="l-item" v-for="(r,i) in rList" :key="i" :class="{active: index == i}"
					@click="getShow(i,r.id)">
					{{r.name}}
				</view>
			</view>
			<view class="r-box">
				<view class="r-list" v-for="(l,i) in rList" :key="i">
					<view class="box">
						<view class="dishes" v-for="(b,i) in l.cs" :key="i">
							<view class="Cuisine" v-if="id == l.id">
								<view class="Cuisine-img" @click="goSearchList(b.name)">
									{{ b.name }}
								</view>
								<view class="Cuisine-list">
									<view class="Cuisine-item" v-for="c in b.cs" :key="c.id"
										@click="goSearchList(c.name)">
										{{ c.name }}
									</view>
								</view>
							</view>
						</view>
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
				rList: [],
				index: 0,
				id: 1
			}
		},
		methods: {
			goSearch() {
				uni.navigateTo({
					url: "/pages/search/search"
				})
			},
			getClassify() {
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url: 'https://apis.netstart.cn/douguo/recipe/flatcatalogs',
					method: 'GET',
					success: (res) => {
						this.rList = res.data.result.cs
						console.log(res.data);
						setTimeout(function() {
							uni.hideLoading();
						}, 100);
					}
				})
			},
			getShow(i, id) {
				this.index = i
				this.id = id
			},
			goSearchList(ks) {
				uni.navigateTo({
					url: '/pages/searchList/searchList?id=' + ks
				})
			}
		},
		mounted() {
			this.getClassify()
		}
	}
</script>

<style lang="scss" scoped>
	.list-search {
		display: flex;
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 50px;
		margin: 0 auto;
		margin-bottom: 10px;
		background-color: #fff;
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

	.l-list {
		width: 90px;
		position: fixed;
		top: 50px;
		left: 0;
		height: 100vh;
		overflow-y: auto;
		text-align: center;

		.l-item {
			height: 35px;
			line-height: 35px;
			background-color: #cccccc24;

			&:last-child {
				padding-bottom: 55px;
			}
		}

		.active {
			background-color: #fff;
			color: #fc0;
		}

		&::-webkit-scrollbar {
			display: none;
		}


	}

	.r-box {
		padding-top: 55px;

		.r-list {
			padding-left: 95px;


			.Cuisine-img {
				margin-bottom: 15px;
				text-align: center;
				font-weight: bold;
			}

			.Cuisine-list {
				display: flex;
				justify-content: space-around;
				flex-wrap: wrap;
				margin-bottom: 15px;

				.Cuisine-item {
					flex: 0 0 20%;
					padding: 3px;
					height: 30px;
					line-height: 30px;
					margin-bottom: 5px;
					margin-right: 6px;
					background-color: #cccccc24;
					text-align: center;
					white-space: nowrap;
				}
			}
		}
	}
</style>
