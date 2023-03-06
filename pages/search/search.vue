<template>
	<view>
		<view class="list-search">
			<view class="search-box">
				<view class="input-box">
					<input type="text" @input="getKs" :value="ks" placeholder="搜你想吃的">
					<view class="search-jin">
						<image src="../../static/img/icon_menu_search.png"></image>
					</view>
				</view>
				<view class="search" @click="goSearchList(ks)">搜索</view>
			</view>
		</view>
		<view class="box" v-if="!ks">
			<view class="lishi">
				<view class="title">
					<view class="lishi-title">
						历史搜索:
					</view>
					<image src="../../static/img/cart_delete.png" @click="del"></image>
				</view>
				<view class="history-list">
					<view class="history-item" v-for="(s,i) in lishi" :key="i" @click="goSearchList(s)">
						{{s}}
					</view>
				</view>
			</view>
			<view class="hot">
				<view class="hot-title">
					热门搜索:
				</view>
				<view class="hot-search">
					<view class="search-category" v-for="(l, i) in searchArr" :key="i" @click="goSearchList(l.title)">
						{{ l.title }}
					</view>
				</view>
			</view>
		</view>
		<view class="list" v-if="ks">
			<view class="item" v-for="(s, i) in searchStrArr" :key="i" @click="goSearchList(s.name)">
				<span class="it-o">{{ s.name }}</span><span class="it-t" v-if="s.tag" :style="{
		          backgroundColor: s.tag.background_color,
		          border: '1px solid s.tag.border_color',
		          borderRadius: '999px',
		          width: '60px',
		          height: '25px',
		          lineHeight: '25px',
		          textAlign: 'center',
		          color: s.tag.text_color,
		        }">{{ s.tag.name }}</span>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ks: '',
				searchArr: [],
				searchStrArr: [],
				lishi: uni.getStorageSync('lishi') || []
			}
		},
		methods: {
			getKs(e) {
				// console.log(e);
				let textKs = e.detail.value
				this.ks = textKs
				uni.request({
					url: 'https://apis.netstart.cn/douguo/recipe/search/suggests?kw=' + this.ks,
					method: 'GET',
					success: (res) => {
						this.searchStrArr = res.data.result.suggests
						console.log(res.data);

					}
				})

			},
			getHot() {
				uni.request({
					url: 'https://apis.netstart.cn/douguo/recipe/search/hot',
					method: 'GET',
					success: (res) => {
						this.searchArr = res.data.result.suggestdetails
						// console.log(res.data);
					}
				})
			},
			goSearchList(ks) {
				let index = this.lishi.findIndex(i => {
					return i == ks
				})
				if (index != -1) {
					this.lishi.splice(index, 1)
				}
				this.lishi = [ks, ...this.lishi]
				// console.log(this.recipt);
				uni.setStorageSync('lishi', this.lishi)
				uni.navigateTo({
					url: '/pages/searchList/searchList?id=' + ks
				})
			},
			del() {
				uni.removeStorageSync("lishi")
				this.lishi = []
			}
		},
		mounted() {
			this.getHot()
		},

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

	.hot-search {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-around;

		.search-category {
			height: 30px;
			margin-top: 10px;
			margin-left: 3px;
			padding: 0 20px;
			line-height: 30px;
			white-space: nowrap;
			text-align: center;
			background: #eeeeee54;
			border-radius: 5px;

			&:nth-child(-n + 5) {
				background: #ffffff;
				color: #e2483c;
				border: 1px solid #e2483c;
			}
		}
	}

	.list {
		list-style: none;
		width: 94vw;
		margin: 0 auto;

		.item {
			display: flex;
			justify-content: space-between;
			width: 100%;
			height: 30px;
			line-height: 30px;
			margin-top: 15px;
			border-bottom: 1px solid #777;
		}
	}

	.lishi {
		margin-bottom: 20px;

		.history-list {
			display: flex;
			flex-wrap: wrap;
			width: 100vw;
			margin-top: 15px;

			.history-item {
				flex: 0 0 25%;
				height: 32px;
				line-height: 32px;
				padding: 0 10px;
				margin-left: 5px;
				margin-top: 5px;
				border-radius: 5px;
				text-align: center;
				background-color: #eee;
				white-space: nowrap;
			}
		}
	}

	.title {
		display: flex;
		justify-content: space-between;
		align-items: center;

		image {
			width: 20px;
			height: 20px;
			margin-right: 5px;
		}
	}

	.lishi-title,
	.hot-title {
		margin-left: 5px;
		font-weight: bold;
	}
</style>
