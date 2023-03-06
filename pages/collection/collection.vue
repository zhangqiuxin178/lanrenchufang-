<template>
	<view>
		<view class="collection-list">
			<view class="edit" v-if="isEdit" @click="showOreHide">
				编辑
			</view>
			<view class="edit" v-else @click="showOreHide">
				完成
			</view>
			<view v-if="collList">
				<view class="list" v-for="(l,i) in collList" :key="i">
					<view class="list-l">
						<image :src="l.img"></image>
					</view>
					<view class="l-box" @click="goDatal(l.id)">
						<view class="list-r">
							<view class="list-title">
								{{l.text}}
							</view>
							<view class="user">
								{{l.user}}
							</view>
							<view class="list-num">
								{{l.vc}} 浏览 {{l.favo_counts}} 收藏
							</view>
						</view>
					</view>
					<view class="ipt" v-if="!isEdit" @click="delColl(l.id)">
						<image src="../../static/img/cart_delete.png"></image>
					</view>
				</view>

			</view>

			<view class="guangguang" v-else>
				<view class="no">
					暂时没有收藏的菜谱
				</view>
				<view class="go">
					去逛逛
				</view>
			</view>
			<view class="end">
				- End -
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				collList: [],
				isEdit: true,
			}
		},
		methods: {
			goDatal(id) {
				uni.navigateTo({
					url: "/pages/home/detail/detail?id=" + id
				})
			},
			showOreHide() {
				this.isEdit = !this.isEdit
			},
			delColl(id) {
				this.collList = this.collList.filter(i => i.id != id)
				uni.setStorageSync('storage_key', this.collList)
			}
		},

		onShow() {
			const arr = uni.getStorageSync('storage_key')
			this.collList = arr
			console.log(arr);

			this.$set(this.collList, 0, arr[0])
			console.log(11);
			this.$forceUpdate();
		}

	}
</script>

<style lang="scss" scoped>
	.ipt {
		flex: 0 0 20px;
		width: 20px;
		height: 20px;
		margin-right: 3px;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.edit {
		margin-top: 5px;
		margin-right: 5px;
		text-align: right;
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
			width: 145px;
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

	.end {
		width: 94vw;
		color: #ccc;
		font-weight: bold;
		font-size: 18px;
		text-align: center;
	}

	.guangguang {
		margin-top: 100px;
		margin-bottom: 25px;
		text-align: center;

		.go {
			width: 60px;
			height: 30px;
			margin: 0 auto;
			background-color: #fc0;
			line-height: 30px;
			color: #fff;
			margin-top: 10px;
			border-radius: 4px;
		}
	}
</style>
