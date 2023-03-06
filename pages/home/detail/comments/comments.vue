<template>
	<view>
		<view class="comment-el" v-if="idnex == 0">
			{{el}}
		</view>

		<view class="comment-list">
			<view class="comment" v-for="(item) in comments" :key="item.id">
				<view class="comments">
					<view class="comments-note">
						<view class="comments-img"><img :src="item.u.p"></view>
						<view class="comments-author">{{item.u.n}}</view>
					</view>
					<view class="comment-img">
						<image src="@/static/img/447cd5605589a7b671e503d29614df7.png"></image>
					</view>
				</view>
				<view class="content-title" v-for="(c,index) in item.content" :key="index">
					<view class="">{{c.c}}</view>
				</view>
			</view>
			<view class="comment" v-for="t in todos" :key="t.id">
				<view class="comments">
					<view class="comments-note">
						<view class="comments-img">
							<image src="../../../../static/img/design_ic_visibility.png"></image>
						</view>
						<view class="comments-author">杰米16686</view>
					</view>
					<view class="comment-img">
						<image src="@/static/img/447cd5605589a7b671e503d29614df7.png"></image>
					</view>
				</view>
				<view class="content-title">
					<view class="">{{t.title}}</view>
				</view>
			</view>

		</view>



		<view class="list-search">
			<view class="search-box">
				<view class="input-box">
					<input type="text" v-model="value" placeholder="我想说两句">
					<view class="search-jin">
						<image src="@/static/img/icon_menu_search.png"></image>
					</view>
				</view>
				<view class="search" @click="addTodo">发表</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				item: '',
				num: 0,
				index: '20',
				el: '',
				comments: '',
				value: '',
				todos: [],
				idnex: 0,
				src: '@/static/img/icon_menu_search.png'
			}
		},
		//
		methods: {
			getflatcomments() {
				uni.request({
					url: 'https://apis.netstart.cn/douguo/recipe/flatcomments/' + this.item + '/' + this.num +
						'/' + this.index,
					data: {
						text: 'uni.request'
					},
					success: (res) => {
						console.log(res.data);
						this.text = 'request success';
						this.el = res.data.result.el;
						this.comments = res.data.result.comments;
						uni.hideLoading();
					}
				});
				uni.showLoading({
					title: '加载中'
				});
			},
			addTodo() {
				this.idnex = 1;
				if (this.value) {
					this.todos.push({
						id: new Date().getTime(),
						title: this.value,
					});
					this.id++;
				}
				this.value = "";
			}

		},
		onLoad(option) {
			console.log(option.id);
			this.item = option.id
		},
		mounted() {
			this.getflatcomments()
		}
	}
</script>

<style lang="scss" scoped>
	.comment-el {
		text-align: center;
		color: #ccc;
	}

	.comment-list {
		padding-bottom: 50px;

		.comment {
			padding: 20px;
			// border-bottom: 2px solid #ccc;


			.content-title {
				margin-left: 25px;
				font-size: 12px;
			}

			.comments {
				display: flex;
				justify-content: space-between;

				.comments-author {
					font-size: 12px;
				}

				.comment-img {
					width: 20px;
					height: 20px;
					margin-right: 20px;

					image {
						display: block;
						width: 100%;
						height: 100%;
					}
				}

				.comments-note {
					display: flex;

					.comments-img {
						width: 25px;
						height: 25px;
						margin-right: 5px;

						image {
							display: block;
							width: 100%;
							height: 100%;
							border-radius: 50%;
						}
					}
				}
			}
		}
	}






	.list-search {
		display: flex;
		width: 100vw;
		height: 50px;
		margin: 0 auto;
		justify-content: center;
		align-items: center;
		position: fixed;
		bottom: 0;
		background-color: #f5f5f5;

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
					background-color: #fff;
					border-radius: 10px;
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
				height: 22px;
				border-bottom-right-radius: 999px;
				border-top-right-radius: 999px;
			}
		}
	}
</style>
