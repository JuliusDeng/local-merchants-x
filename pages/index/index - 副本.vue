<template>
	<view>
		<!-- navbar -->
		<u-navbar :is-back="false">
			<view class="search">
				<view class="search-text">{{ baseInfo.site_name }}</view>
				<u-search placeholder=" " :show-action="false" disabled height="56" @click="toRoute({
            url: 'pages/index/search'
          })"></u-search>
			</view>
		</u-navbar>
		<!-- tips -->
		<struggler-uniapp-add-tip></struggler-uniapp-add-tip>
		<!-- tabs -->
		<u-sticky :offset-top="offsetTop">
			<view class="tabs">
				<u-tabs name="cate_name" :list="tabsList" :current="tabsCurrent" @change="tabsChange"></u-tabs>
			</view>
		</u-sticky>
		<!-- top -->
		<view class="top-box">
			<image src="/static/topbg.jpg" mode="aspectFill" class="image"></image>
		</view>
		<!-- topradius -->
		<view class="top-radius">

		</view>
		<!-- banner -->
		<!-- <view class="banner">
      <u-swiper name="pic" :list="banner" border-radius="30" mode="none" 
        @change="changeSwiper" @click="toSwiperPath"></u-swiper>
      <view class="d-botbox">
        <view v-for="(item, index) in banner" :key="index" class="b-dot" :class="[index == swiperCurrent ? 'active' : '']"></view>
      </view>
    </view> -->
		<template v-if="!tabsCurrent">
			<!-- grid -->
			<view class="grid-box">
				<u-grid :col="4" :border="false">
					<u-grid-item v-for="(item, index) in gridList" :key="index" @click="toRoute({
              url: item.url,
              params: {
                title: item.name
              }
            })">
						<u-icon :name="item.pic" :size="56"></u-icon>
						<view class="grid-text">{{ item.name }}</view>
					</u-grid-item>
				</u-grid>
			</view>
			<!-- 今日秒杀 -->
			<view class="new-goods" v-if="nowStep > -1 && footMoreList[0].length">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top"></view>
					<text>⭐️ 今日商品秒杀 ❥</text>
					<!-- <text class="label">每天9:30，准时开售</text> -->
					<view class="sk-more">
						<navigator url="../goods/seckill">
							查看更多<u-icon name="arrow-right" size="24"></u-icon>
						</navigator>
					</view>
				</view>
				<view class="new-goods-list">
					<view class="new-goods-item" v-for="(item, index) in footMoreList[0]" :key="index" @click="toDetails(item, 1, datatime)">
						<advance-item :src="item.image" :title="item.store_name" :price="item.price" :show-old="false" :timestamp="datatime"
						 @handle="toDetails(item, 1, datatime)">
						</advance-item>
					</view>
					<view class="look-more">
						<navigator url="../goods/seckill" class="ain">查看更多</navigator>
					</view>
				</view>
			</view>

			<!-- 天天爱抽奖 -->
			<view class="">
				天天爱抽奖
			</view>


			<!-- 广告图 商圈查看 -->
			<view class="look-district">
				<!-- <view class="title-box">
          <text class="text">一键查看所有商圈</text>
        </view> -->
				<view class="lookwrapbox" v-if="hotMoreGrid.length == 3 || hotMoreGrid.length == 1 || hotMoreGrid.length == 6 || hotMoreGrid.length == 5">
					<view class="lookitem" v-for="(item, index) in hotMoreGrid.slice(0, 3)" :key="index" :class="[hotMoreGrid.length == 1 ? 'look-one' : '']"
					 @click="toRoute({
              url: item.url
            })">
						<u-image :src="item.pic" width="100%" height="100%" mode="scaleToFill"></u-image>
						<!-- <text class="text">{{ item.title }}</text> -->
					</view>
				</view>
				<view class="lookwrapbox" v-if="hotMoreGrid.length == 2 || hotMoreGrid.length == 4 || hotMoreGrid.length == 5 ">
					<view class="lookitem look-two" v-for="(item, index) in (hotMoreGrid.length == 5 ? hotMoreGrid.slice(3) : hotMoreGrid)"
					 :key="index" :class="[hotMoreGrid.length == 1 ? 'look-one' : '']" @click="toRoute({
              url: item.url
            })">
						<u-image :src="item.pic" width="100%" height="100%" mode="scaleToFill"></u-image>
						<!-- <text class="text">{{ item.title }}</text> -->
					</view>
				</view>
				<view class="lookwrapbox" v-if="hotMoreGrid.length == 6">
					<view class="lookitem" v-for="(item, index) in hotMoreGrid.slice(3)" :key="index" @click="toRoute({
              url: item.url
            })">
						<u-image :src="item.pic" width="100%" height="100%" mode="scaleToFill"></u-image>
						<!-- <text class="text">{{ item.title }}</text> -->
					</view>
				</view>
			</view>
			<!-- 吃喝玩乐推荐 card swiper -->
			<view class="card-swiper">
				<view class="card-top">
					<view class="t-left">
						<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
						<text class="title">⭐️ 吃喝玩乐推荐</text>
						<view class="line"></view>
						<!-- <text class="label">更好的时代，值得遇见更好的你</text> -->
					</view>
					<view class="t-right">
						<text class="bold">{{ cardSwiper+1 }}</text><text class="text">/{{ footMoreList[1].length }}</text>
					</view>
				</view>
				<view class="card-bot">
					<swiper previous-margin="30rpx" next-margin="30rpx" style="height: 450rpx" :current="cardSwiper" @change="changeCardSwiper">
						<swiper-item v-for="(item, index) in footMoreList[1]" :key="index" @click="toRoute({
                url: 'pages/goods/details',
                params: {
                  id: item.product_id
                }
              })">
							<view class="cardbox">
								<u-image :src="item.image" width="100%" height="350rpx" border-radius="10" mode="scaleToFill"></u-image>
								<view class="card-info">
									<text class="text u-line-1">{{ item.store_name }}</text>
									<text class="label u-line-1">{{ item.store_info }}</text>
								</view>
							</view>
						</swiper-item>
					</swiper>
				</view>
			</view>
			<!-- 玩乐特惠专区 精品推荐 -->
			<view class="tabulation-box" v-if="nowStep > 1">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top" :style="{width: 56 * 5 + 'rpx'}"></view>
					<text class="text">⭐️ 玩乐特惠专区</text>
					<view class="secmore" @click="toRoute({
             url: 'pages/goods/list', 
             params: {
               title: '⭐️ 玩乐特惠专区',
               isvip: '1'
             }
           })">
						<text>+更多推荐</text>
					</view>
				</view>
				<!-- <view class="tabulation-image">
         <u-image 
           src="https://img.zcool.cn/community/01561e5f82aaf911013e458429655e.jpg@1280w_1l_2o_100sh.jpg" 
           width="100%" height="300rpx" border-radius="10">
         </u-image>
       </view> -->
				<!-- <view class="tabulation-more">
         <u-section title="美食从来不说谎" sub-title="+更多推荐" :show-line="false" :bold="false" font-size="36"
           @click="toRoute({
             url: 'pages/goods/list',
             params: {
               type: 0,
               title: '美食从来不说谎'
             }
           })">
         </u-section>
       </view> -->
				<view class="goods-inbox">
					<view class="goods-initem" v-for="(item, index) in footMoreList[2]" :key="index">
						<goods-item :src="item.image" :title="item.store_name" :price="item.price" o-price=" " :show-btn="false"
						 :right-text="`已售 ${item.sales}`" @click="toDetails(item)">
						</goods-item>
					</view>
				</view>
			</view>
			<!-- 本周上新 -->
			<view class="goods-list" v-if="nowStep > 2">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top" style="width: 370rpx;"></view>
					<text>⭐️ 网红风味|吃货必备</text>
					<view class="secmore" @click="toRoute({
              url: 'pages/goods/list', 
              params: {
                title: '➡️向右滑动查看',
                isvip: '1'
              }
            })">
						<text>+更多推荐</text>
					</view>
				</view>
				<scroll-view scroll-x="true">
					<view class="goods-list-box">
						<view class="goods-listitem u-padding-right-10" v-for="(item, index) in footMoreList[3]" :key="index">
							<goods-item :src="item.image" :title="item.store_name" :price="item.price" o-price=" " @click="toDetails(item)">
							</goods-item>
						</view>
					</view>
				</scroll-view>
			</view>
			<!-- 本周上新 -->
			<view class="goods-list" v-if="nowStep > 3">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top" style="width: 330rpx;"></view>
					<text>⭐️ 火锅|一起涮火锅</text>
				</view>
				<scroll-view scroll-x="true">
					<view class="goods-list-box">
						<view class="goods-listitem u-padding-right-10" v-for="(item, index) in footMoreList[4]" :key="index">
							<goods-item :src="item.image" :title="item.store_name" :price="item.price" o-price=" " @click="toDetails(item)">
							</goods-item>
						</view>
					</view>
				</scroll-view>
			</view>
			<!-- 本周上新 -->
			<view class="goods-list" v-if="nowStep > 4">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top" style="width: 410rpx;"></view>
					<text>⭐️ 烤肉|烤鱼|烧烤吃不腻</text>
				</view>
				<scroll-view scroll-x="true">
					<view class="goods-list-box">
						<view class="goods-listitem u-padding-right-10" v-for="(item, index) in footMoreList[5]" :key="index">
							<goods-item :src="item.image" :title="item.store_name" :price="item.price" o-price=" " @click="toDetails(item)">
							</goods-item>
						</view>
					</view>
				</scroll-view>
			</view>
			<!-- 广告 -->
			<view class="advint" v-if="nowStep > 4 && ad.home_ad_pic" @click="toRoute({url: ad.home_ad_url})">
				<u-image :src="ad.home_ad_pic" width="100%" height="200rpx"></u-image>
			</view>

			<!-- 吃喝玩乐 -->
			<view class="tabulation-box" v-if="nowStep > 5">
				<view class="title-box">
					<!-- <image src="/static/ico-star.png" mode="aspectFit" class="ico-star"></image> -->
					<view class="line line-top" :style="{width: 56 * 6.5 + 'rpx'}"></view>
					<text class="text">⭐️ 必吃美食|优选Top</text>
					<view class="secmore" @click="toRoute({
              url: 'pages/goods/list', 
              params: {
                title: '⭐️ 必吃美食|优选Top',
                hot_type: 'good'
              }
            })">
						<text>+更多推荐</text>
					</view>
				</view>
				<!-- <view class="tabulation-image">
          <u-image 
            src="https://img.zcool.cn/community/0137ec5c40168aa801203d2263e5f1.jpg@260w_195h_1c_1e_1o_100sh.jpg" 
            width="100%" height="300rpx" border-radius="10"
            >
          </u-image>
        </view> -->
				<!-- <view class="tabulation-more">
          <u-section title="解忧放松好去处" sub-title="+更多推荐" :show-line="false" :bold="false" font-size="36"
            @click="toRoute({
              url: 'pages/goods/list',
              params: {
                type: 1,
                title: '解忧放松好去处'
              }
            })">
          </u-section>
        </view> -->
				<view class="goods-inbox">
					<view class="goods-initem" v-for="(item, index) in footMoreList[6]" :key="index">
						<goods-item :src="item.image" :title="item.store_name" :price="item.price" o-price=" " :show-btn="false"
						 :right-text="`已售 ${item.sales}`" @click="toDetails(item)">
						</goods-item>
					</view>
				</view>
			</view>
			<!-- foot logo -->
			<view class="logo-box" v-if="indexLoadStatus == 'nomore'">
				<!-- <image src="/static/logo.png" mode="aspectFit" class="img-logo"></image> -->
				<text class="text">{{ baseInfo.site_name }}技术支持</text>
			</view>
			<!-- loadmore -->
			<view class="u-padding-20" v-else>
				<u-loadmore :status="indexLoadStatus"></u-loadmore>
			</view>
		</template>
		<template v-else>
			<view class="goods-inbox">
				<view class="goods-initem" v-for="(item, index) in list" :key="index">
					<goods-item :src="item.image" :title="item.store_name" :price="item.price" :o-price="item.ot_price" :show-btn="false"
					 :right-text="`已售 ${item.sales}`" @click="toDetails(item)">
					</goods-item>
				</view>
			</view>
			<view class="u-padding-20">
				<u-loadmore :status="loadStatus"></u-loadmore>
			</view>
		</template>
		<!-- mask -->
		<u-mask :show="showReindex && footMoreList[1]">
			<view class="re-index">
				<image src="/static/fixbg.png" mode="aspectFit" class="re-bg"></image>
				<view class="re-content">
					<view class="re-item">
						<view class="re-img">
							<u-image :src="footMoreList[1][0].image" width="100%" height="100%" />
						</view>
						<view class="re-right">
							<text class="re-title u-line-2">{{ footMoreList[1][0].store_name }}</text>
							<view class="re-bot">
								<text class="re-price">¥{{ footMoreList[1][0].price }}</text>
								<text class="re-small">已有{{ footMoreList[1][0].sales }}人抢购</text>
							</view>
						</view>
					</view>
				</view>
				<view class="button-text" @click="toDetails(footMoreList[1][0])">
					<text>立即抢购</text>
				</view>
				<view class="re-hide" @click="showReindex = false">
					<u-icon name="close-circle" size="60" color="rgba(255, 255, 255, .8)"></u-icon>
				</view>
			</view>
		</u-mask>
	</view>
</template>

<script>
	const appSystem = uni.getSystemInfoSync()
	export default {
		data() {
			return {
				hotMoreGrid: [{
					text: '爆品专区',
					image: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1021385598,3332745907&fm=15&gp=0.jpg',
					path: 'pages/goods/list?hot_type=hot'
				}],
				showReindex: true,
				tabsList: [{
					cate_name: '推荐'
				}, ],
				tabsCurrent: 0,
				banner: [],
				swiperCurrent: 0,
				cardSwiper: 0,
				gridList: [],
				page: 1,
				limit: 10,
				loadStatus: 'loadmore',
				indexLoadStatus: 'loadmore',
				list: [],
				recommendList: [],
				nowStep: -1,
				footMoreList: [],
				datatime: 0,
				init: true,
				ad: {}
			}
		},
		computed: {
			baseInfo() {
				return this.$store.state.baseInfo
			},
			offsetTop() {
				let scale = 750 / appSystem.screenWidth
				// #ifndef H5
				return (appSystem.statusBarHeight + 44) * scale
				// #endif
				// #ifdef H5
				return 0
				// #endif
			}
		},
		onLoad() {
			// 获取数据
			this.getIndexData()
			if (this.indexLoadStatus == 'loadmore') {
				this.indexLoadStatus = 'loading'
				this.getFootMore()
			}
			this.getSeckillTime()
		},
		onReachBottom() {
			if (!this.tabsCurrent) { // 首页index
				if (this.indexLoadStatus == 'loadmore') {
					this.indexLoadStatus = 'loading'
					this.getFootMore()
				}
			} else { // 列表list
				this.getList()
			}
		},
		methods: {
			toDetails(item, type = 0, date) {
				this.toRoute({
					url: 'pages/goods/details',
					params: {
						type: type,
						id: item.product_id,
						date: date
					}
				})
			},
			// 获取秒杀截止时间
			getSeckillTime() {
				this.$u.get('/api/store/product/seckill/select').then(res => {
					this.datatime = res.data.seckillEndTime;
				});
			},
			// 获取推荐商品
			getFootMore() {
				let url = '/api/store/product/recommend/lst'
				let limit = 4
				let params = {}
				switch (this.nowStep) {
					case -1: // 秒杀
						url = '/api/store/product/seckill/lst'
						break;
					case 0: // 吃喝玩乐推荐
						url = '/api/store/product/lst'
						params.hot_type = 'good'
						break;
					case 1:
						url = '/api/store/product/lst'
						params.isvip = '1'
						break;
					case 2: // 本周上新
						url = '/api/store/product/lst'
						params.cate_id = 314
						// params.hot_type = 'new'
						limit = 10
						break;
					case 3: // 火锅
						url = '/api/store/product/lst'
						params.hot_type = 'new'
						limit = 10
						// this.nowStep ++
						// this.getFootMore()
						// return null;
						break;
					case 4: // 烧烤
						url = '/api/store/product/lst'
						params.hot_type = 'new'
						limit = 10
						// this.nowStep ++
						// this.getFootMore()
						// return null;
						break;
					case 5:
						url = '/api/store/product/lst'
						params.hot_type = 'best'
						break;
					default:
						break;
				}
				this.$u.get(url, {
					page: 1,
					limit: limit,
					...params
				}).then(({
					data
				}) => {
					this.nowStep++
					this.footMoreList[this.nowStep] = data.list
					if (this.nowStep > 6) {
						this.indexLoadStatus = 'nomore'
					} else {
						this.indexLoadStatus = 'loadmore'
					}
					if (this.init) {
						this.init = false
						this.getFootMore()
					}
				})
			},
			// 获取除首页外列表数据
			getList() {
				if (this.tabsCurrent && this.loadStatus == 'loadmore') {
					this.loadStatus = 'loading'
					let pid = this.tabsList[this.tabsCurrent].store_category_id
					this.$u.get('/api/store/product/lst', {
						cate_id: pid,
						page: this.page++,
						limit: this.limit
					}).then(({
						data
					}) => {
						this.list = this.list.concat(data.list)
						if (data.list.length < this.limit) {
							this.loadStatus = 'nomore'
						} else {
							this.loadStatus = 'loadmore'
						}
					})
				}
			},
			// 获取首页数据
			getIndexData() {
				this.$u.get('/api/common/home')
					.then(({
						data
					}) => {
						this.ad = data.ad
						this.banner = data.banner
						// this.recommendList = data.hot
						this.hotMoreGrid = data.hot
						this.tabsList = this.tabsList.concat(data.category)
						this.gridList = this.gridList.concat(data.menu)
					})
			},
			// banner 点击跳转对应页面
			toSwiperPath(index) {
				let url = this.banner[index].url
				if (url) {
					this.$u.route({
						url: url
					})
				}
			},
			// 路由跳转
			toRoute(params) {
				this.$u.route(params)
			},
			// 卡片切换
			changeCardSwiper({
				detail
			}) {
				this.cardSwiper = detail.current
			},
			// tabs切换
			tabsChange(index) {
				this.tabsCurrent = index
				this.page = 1
				this.loadStatus = 'loadmore'
				this.list = []
				this.getList()
			},
			// swiper banner 切换
			changeSwiper(index) {
				this.swiperCurrent = index
			}
		}
	}
</script>

<style lang="scss">
	.search {
		padding-right: 50rpx;
		display: flex;
		align-items: center;
		height: 100%;

		.search-text {
			font-size: $uni-font-size-base;
			padding: 0 $gutter;
			width: 250rpx;
		}
	}

	.banner {
		padding: $gutter;
		position: relative;
		min-height: 250rpx;

		.d-botbox {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: center;
			padding-top: $gutter;

			.b-dot {
				width: 16rpx;
				height: 4rpx;
				background-color: rgba($color: #000000, $alpha: 0.3);

				&.active {
					background-color: rgba($color: #000000, $alpha: 0.8);
				}
			}
		}

		&::before {
			content: '';
			position: absolute;
			left: 0;
			top: 0;
			width: 100%;
			height: 200rpx;
			border-bottom-left-radius: 50%;
			border-bottom-right-radius: 50%;
			background-color: $uni-color-warning;
		}
	}

	.card-swiper {
		position: relative;

		.card-top {
			display: flex;
			padding: $gutter;

			.t-left {
				flex: 1;
				display: flex;
				flex-direction: column;
				position: relative;
				padding-left: 16rpx;

				.title {
					font-size: 36rpx;
					padding-bottom: 10rpx;
				}

				.label {
					font-size: $uni-font-size-sm;
				}
			}

			.t-right {
				display: flex;
				align-items: flex-end;
				font-size: $uni-font-size-sm;
				color: #999;
				padding-bottom: 2rpx;

				.bold {
					font-size: $uni-font-size-xl;
					color: #333;
					font-weight: bold;
				}

				.text {
					padding-bottom: 6rpx;
				}
			}
		}

		.card-bot {
			position: relative;
			margin-left: -$gutter;

			.cardbox {
				display: flex;
				flex-direction: column;
				padding: 0 $gutter / 2;

				.card-info {
					padding: 10rpx 0;
					display: flex;
					flex-direction: column;

					.text {
						font-size: $uni-font-size-lg;
						padding-bottom: 10rpx;
					}

					.label {
						font-size: $uni-font-size-sm;
						color: #999;
					}
				}
			}
		}
	}

	.grid-box {
		position: relative;

		.grid-text {
			font-size: $uni-font-size-sm;
		}
	}

	.title-box {
		position: relative;
		padding: $gutter;
		font-size: $uni-font-size-xl;
		color: #333;
		display: flex;
		flex-direction: column;
		padding-left: 10rpx;

		.label {
			font-size: $uni-font-size-sm;
			color: #999;
			margin-top: 10rpx;
		}

		.sk-more {
			position: absolute;
			right: $gutter;
			top: 50%;
			z-index: 10;
			color: #666;
			margin-top: -12rpx;
			font-size: 24rpx;
		}
	}

	.goods-list {
		position: relative;

		.goods-list-box {
			display: flex;
			align-items: center;
			padding: $gutter;
		}

		.goods-listitem {
			min-width: 300rpx;
			max-width: 300rpx;
		}
	}

	.goods-inbox {
		position: relative;
		display: flex;
		flex-wrap: wrap;

		.goods-initem {
			width: 345rpx;
			margin-left: $gutter;
			margin-bottom: $gutter;
		}
	}

	.new-goods-list {
		display: flex;
		flex-direction: column;

		.new-goods-item {
			padding: $gutter;
		}
	}

	.look-district {
		display: flex;
		flex-direction: column;
		background-color: $bg;
		padding-top: 20rpx;

		.lookwrapbox {
			display: flex;
			flex-wrap: wrap;

			.lookitem {
				width: 345rpx;
				height: 240rpx;
				position: relative;
				border-radius: $uni-border-radius-lg;
				overflow: hidden;
				margin-left: $gutter;
				margin-bottom: $gutter;

				&:nth-child(3n-2) {
					height: 500rpx;

					&.look-two {
						height: 240rpx;
					}
				}

				&:nth-child(3n) {
					margin-top: -260rpx;
					margin-left: auto;
					margin-right: 24rpx;

					&.look-two {
						margin-top: 0;
						margin-left: $gutter;
						margin-right: 0;
					}
				}

				&.look-one {
					width: 710rpx;
					height: 300rpx;
				}

				.text {
					position: absolute;
					left: 50%;
					top: 50%;
					transform: translate(-50%, -50%);
					z-index: 10;
					color: #fff;
					font-size: $uni-font-size-xl;
					font-weight: bold;
					white-space: nowrap;
				}

				// &::after {
				//   content: '';
				//   position: absolute;
				//   left: 0;
				//   top: 0;
				//   width: 100%;
				//   height: 100%;
				//   background-color: rgba($color: #000000, $alpha: 0.6);
				// }
			}
		}
	}

	.tabulation-box {
		position: relative;

		.tabulation-image {
			padding: 0 $gutter;
		}

		.tabulation-more {
			padding: $gutter;
			background-color: rgba($color: #ffdd00, $alpha: 0.2);
			margin: $gutter;
			border-top-right-radius: $uni-border-radius-lg;
			border-top-left-radius: $uni-border-radius-lg;
			overflow: hidden;
		}
	}

	.logo-box {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		padding: $gutter;

		.img-logo {
			width: 60rpx;
			height: 60rpx;
		}

		.text {
			font-size: 24rpx;
			color: #999;
			line-height: 1.5em;
			margin-top: 10rpx;
		}
	}

	.look-more {
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 24rpx;
		color: #666;
		padding: $gutter 0 30rpx;

		.ain {
			width: 100%;
			text-align: center;
		}
	}

	.top-box {
		height: 240rpx;
		width: 100%;
		background-color: #FFDD00;
		padding-bottom: 40rpx;
	}

	.top-radius {
		border-top-right-radius: 20rpx;
		border-top-left-radius: 20rpx;
		height: 30rpx;
		background-color: #fff;
		margin-top: -26rpx;
		position: relative;
		z-index: 10;
	}

	.re-index {
		width: 100%;
		height: 500rpx;
		position: absolute;
		left: 0;
		top: 50%;
		transform: translateY(-40%);
		z-index: 9999;

		.re-content {
			height: 400rpx;
			z-index: 10;
			position: relative;
			overflow: hidden;

			.re-item {
				width: 360rpx;
				margin: 105rpx auto 0;
				background-color: #fff;
				padding: 10rpx;
				border-radius: 10rpx;
				border: 4rpx solid #FFDD00;
				box-shadow: 0 0 5rpx rgba($color: #000000, $alpha: 0.3);
				display: flex;

				.re-img {
					min-width: 120rpx;
					width: 120rpx;
					height: 110rpx;
				}

				.re-right {
					margin-left: auto;
					width: 200rpx;
					max-width: 200rpx;
					min-width: 200rpx;
					font-size: 24rpx;
					display: flex;
					flex-direction: column;

					.re-bot {
						margin-top: auto;
						font-size: 16rpx;
						display: flex;
						justify-content: space-between;

						.re-price {
							color: red;
							font-size: 20rpx;
						}

						.re-small {
							color: #999;
						}
					}
				}
			}
		}

		.button-text {
			position: absolute;
			left: 0;
			bottom: 136rpx;
			font-size: 36rpx;
			color: #844216;
			z-index: 10;
			width: 100%;
			text-align: center;
			padding-left: 30rpx;
			height: 70rpx;
			line-height: 70rpx;
		}

		.re-bg {
			width: 100%;
			height: 450rpx;
			position: absolute;
			top: -50rpx;
			left: 0;
			z-index: 0;
		}

		.re-hide {
			width: 100%;
			height: 100rpx;
			display: flex;
			align-items: center;
			justify-content: center;
			position: relative;
			z-index: 2;
		}
	}

	.ico-star {
		position: absolute;
		left: 0;
		top: 50%;
		margin-top: -40rpx;
		width: 80rpx;
		height: 80rpx;
		z-index: 10;
	}

	.line {
		position: absolute;
		left: 10rpx;
		bottom: 10rpx;
		width: 260rpx;
		height: 16rpx;
		background-color: #fee233;
		z-index: -1;
		border-radius: 10rpx;

		&.line-top {
			width: 260rpx;
			bottom: 30rpx;
		}
	}

	.secmore {
		position: absolute;
		right: 20rpx;
		top: 50%;
		transform: translateY(-50%);
		font-size: 24rpx;
		z-index: 10;
		color: #666;
	}

	.advint {
		padding: $gutter;
	}
</style>
