<template>
	<view>
		<view class="detail-box">
			<view class="detali-img">
				<image :src="detailObj.photo_path"></image>
			</view>
			<view class="detail-title">
				{{detailObj.title}}
			</view>
			<view class="detail-num">
				{{detailObj.vc}}人浏览·{{detailObj.favo_counts}}人收藏
			</view>
			<view class="detail-content">
				{{detailObj.cookstory}}
			</view>
			<view class="user-box">
				<view class="user-img">
					<image :src="detailObj.user.avatar_medium"></image>
				</view>
				<view class="user-name">
					{{detailObj.user.nick}}
				</view>
			</view>

			<view class="listing-box">
				<view class="listing-title">
					- 食材清单 -
				</view>
				<view class="listing-list" v-for="(l,i) in detailObj.major" :key="i">
					<view class="item-l">
						{{l.title}}
					</view>
					<view class="item-r">
						{{l.note}}
					</view>
				</view>
			</view>

			<view class="steps-box">
				<view class="steps-title">
					- 烹饪步骤 -
				</view>
				<view class="steps-list" v-for="(s,i) in detailObj.cookstep" :key="i">
					<view class="steps-img">
						<image :src="s.image"></image>
					</view>
					<view class="steps-item">
						{{s.position}}. {{s.content}}
					</view>
				</view>
			</view>

			<view class="related-box">
				<view class="related-title">
					- 小贴士 -
				</view>
				<view class="tips">
					{{detailObj.tips}}
				</view>
			</view>

			<view class="end">
				- End -
			</view>
		</view>
		<view class="foot-bar">
			<view class="bar-item" @click="goHome">
				首页
			</view>
			<view class="bar-item" @click="goComments">
				评论
			</view>
			<view class="bar-item" v-if="isShow" @click="setcommentStorage()">
				收藏
			</view>
			<view class="bar-item" v-else @click="setcommentStorage()">
				已收藏
			</view>
			<view class="bar-item" data-name="shareBtn" open-type="share">
				分享
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				itemId: '',
				detailObj: {},
				relatedList: [],
				recipt: uni.getStorageSync('storage_key') || [],
				index: 0,
				isShow: true,
				arr: [],
				newObj: {}
			}
		},
		methods: {
			getDetail() {
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url: "https://apis.netstart.cn/douguo/recipe/detail/" + this.itemId,
					method: 'GET',
					success: (res) => {
						this.detailObj = res.data.result?.recipe
						// console.log(this.detailObj);
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
			goHome() {
				uni.switchTab({
					url: "/pages/home/home"
				})
			},
			goComments() {
				// console.log(111);
				uni.navigateTo({
					url: '/pages/home/detail/comments/comments?id=' + this.itemId
				})
			},
			setcommentStorage() {
				this.isShow = !this.isShow;
				this.newObj = {
					text: this.detailObj.title,
					user: this.detailObj.user.nickname,
					id: this.detailObj.cook_id,
					img: this.detailObj.photo_path,
					finish: false,
					vc: this.detailObj.vc,
					favo_counts: this.detailObj.favo_counts
				}

				if (this.isShow === false) {
					// 查找 id 是否相等
					let index = this.recipt.findIndex(i => {
						return i.id == this.newObj.id
					})
					// index != -1 已存在 删除
					if (index != -1) {
						this.recipt.splice(index, 1)
					}
					// 添加
					this.recipt = [this.newObj, ...this.recipt]
					// console.log(this.recipt);
					// 存入本地数据
					uni.setStorageSync('storage_key', this.recipt)
					uni.showToast({
						title: '已收藏',
						icon: 'none',
						duration: 1000
					});
				} else {
					// 过滤id不相等的 
					this.recipt = this.recipt.filter(i => i.id != this.newObj.id)
					// 覆盖 取消收藏
					uni.setStorageSync('storage_key', this.recipt)
					// console.log(this.recipt);
					uni.showToast({
						title: '取消收藏',
						icon: 'none',
						duration: 1000
					});
				}
			},
			showOreHide() {
				this.recipt.forEach(i => {
					if (i.id == this.itemId) {
						// console.log(1, this.isShow = );
						this.isShow = !this.isShow
					} else {
						this.isShow = this.isShow
						// console.log(2);
					}
				})
			},
		},
		onLoad(option) { //option为object类型，会序列化上个页面传递的参数
			this.itemId = option.id,
				//设置下方的Menus菜单，才能够让发送给朋友与分享到朋友圈两个按钮可以点击
				wx.showShareMenu({
					withShareTicket: true,
					menus: ["shareAppMessage", "shareTimeline"]
				})
		},
		mounted() {
			this.getDetail()
			this.showOreHide()
		},

		//发送给朋友
		onShareAppMessage(res) {
			return {
				title: this.detailObj.title,
				path: "pages/home/home"
			}
		},
		//分享到朋友圈
		onShareTimeline(res) {
			return {
				title: this.detailObj.title,
				type: 0,
				query: 0,
				summary: "",
				imageUrl: this.detailObj.photo_path
			}
		},
	}
</script>

<style lang="scss" scoped>
	.detail-title {
		width: 94vw;
		margin: 10px auto;
		font-size: 20px;
		font-weight: bold;
		text-align: center;
	}

	.detail-num {
		width: 94vw;
		margin: 10px auto;
		color: #666;
		text-align: center;
		font-size: 12px;
	}

	.detail-content {
		width: 94vw;
		margin: 10px auto;
		white-space: pre-wrap;
		white-space: -moz-pre-wrap;
		white-space: -pre-wrap;
		white-space: -o-pre-wrap;
	}

	.user-box {
		display: flex;
		align-items: center;
		width: 94vw;
		margin: 10px auto;

		.user-img {
			width: 30px;
			height: 30px;
			border-radius: 999px;
			overflow: hidden;


			image {
				width: 100%;
				height: 100%;
			}
		}

		.user-name {
			margin-left: 5px;
		}
	}

	.listing-box {
		margin-bottom: 30px;

		.listing-title {
			width: 94vw;
			margin: 10px auto;
			text-align: center;
			font-size: 20px;
			font-weight: bold;
		}

		.listing-list {
			display: flex;
			justify-content: space-between;
			width: 94vw;
			height: 30px;
			line-height: 30px;
			margin: 10px auto;
			border-bottom: 1px solid #ccc;
		}
	}

	.steps-box {
		margin-bottom: 30px;

		.steps-title {
			width: 94vw;
			margin: 10px auto;
			text-align: center;
			font-size: 20px;
			font-weight: bold;
		}

		.steps-list {
			margin-bottom: 15px;

			.steps-item {
				width: 94vw;
				margin: 10px auto;

			}
		}
	}

	.related-box {
		margin-bottom: 30px;

		.related-title {
			width: 94vw;
			margin: 10px auto;
			text-align: center;
			font-size: 20px;
			font-weight: bold;
		}

		.tips {
			width: 94vw;
			margin: 10px auto;
		}
	}

	.end {
		width: 94vw;
		margin: 10px auto;
		color: #ccc;
		text-align: center;
		font-size: 20px;
		font-weight: bold;
		padding-bottom: 50px;
	}

	.foot-bar {
		display: flex;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100vw;
		height: 45px;
		background-color: #fff;

		.bar-item {
			flex: 0 0 25%;
			height: 45px;
			line-height: 45px;
			text-align: center;
		}
	}
</style>
