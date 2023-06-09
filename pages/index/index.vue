<template>
	<view class="content">
		<view class="top-tabs">
			<u-tabs lineWidth="40" lineColor="#FB7299" 
			:current="navAction" 
			:list="navList" 
			:activeStyle="activeStyle" 
			:inactiveStyle="inactiveStyle" 
			@click="clickNav">
			</u-tabs>
		</view>
		<view class="uni-swiper">
			<swiper class="swiper-box"
				:current="navAction"
				:duration="300" 
				@transition="onswiperscroll" 
				@animationfinish="animationfinish" >
				<swiper-item class="swiper-item" v-for="(page, index) in navList" :key="index">
					<posts-page class="page-item" :cateId="page._id" :ref="'page' + index"></posts-page>
				</swiper-item>
			</swiper>
		</view>
		
		
		
	</view>
</template>

<script>
	const db = uniCloud.database()
	const TAB_PRELOAD_OFFSET = 1;
	export default {
		data() {
			return {
				navList: [],
				navAction: 0,
				pageList: [],
				activeStyle: {
					color: '#303133',
					fontWeight: 'blod',
					transform: 'scale(1.08)'
				},
				inactiveStyle: {
					color: '#999999',
					transform: 'scale(1)'
				},
				isTap: false	//是否通过点击切换页面
			}
		},
		onLoad() {
			this.getCategory()
		},
		mounted() {
			
		},
		methods: {
			//点击tabs标签
			clickNav(e) {
				// this.loadState = true
				// this.loadMoreState = 'more'
				// this.blogList = []
				
				//更新当前索引值
				this.navAction = e.index
				//设置通过点击切换页面
				this.isTap = true
				
				
				// this.getData()
			},
			//预加载滑动到的下一个 tab 页面的数据
			onswiperscroll(e) {
				//如果是点击操作则直接返回，不进行预加载
				if (this.isTap) {
					return;
				}
				
				//获取滑动的偏移量
				var offsetX = e.detail.dx;
				//计算出预加载的 tab 页面的索引
				var preloadIndex = this.navAction;
				if (offsetX > TAB_PRELOAD_OFFSET) {
					preloadIndex++;		//右滑
				} else if (offsetX < -TAB_PRELOAD_OFFSET) {
					preloadIndex--;		//左滑
				}
				//索引没变或索引溢界则不进行预加载
				if (preloadIndex === this.navAction || preloadIndex < 0 || preloadIndex > this.pageList.length - 1) {
					return;
				}
				//判断预加载的 tab 页面是否已经加载过数据
				// if (this.pageList[preloadIndex].dataList.length === 0) {
				// 	this.loadTabData(preloadIndex);
				// }
			},
			animationfinish(e) {
				this.navAction = e.detail.current;
				this.isTap = false;
			},
			async getCategory() {
				const cateRes = await db.collection('post-categories')
					.where('status == 1')
					.field('title,icon')
					.orderBy('sort asc')
					.get()
					
				console.log("getCategory:",cateRes.result.data)
				this.navList = cateRes.result.data.map(({_id,title: name,icon}) => ({_id,name,icon}))
						
				await this.$nextTick()
				for (var i = 0; i < this.navList.length; i++) {
					let item = this.$refs['page' + i]
					if (Array.isArray(item)) {
						this.pageList.push(item[0])
					} else {
						this.pageList.push(item)
					}
				}
			},
			
		}
	}
</script>

<style>
	.content {}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>