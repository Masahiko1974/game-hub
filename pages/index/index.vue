<template>
	<view class="content">
		<view class="top-tabs">
			<u-tabs :list="navList" lineWidth="40" lineColor="#FB7299" :activeStyle="activeStyle"
				:inactiveStyle="inactiveStyle" @click="clickNav">
			</u-tabs>
		</view>
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				navList: [{
					name: '最新',
					type: 'publish_date'
				}, {
					name: '热门',
					type: 'view_count'
				}],
				navAction: 0,
				activeStyle: {
					color: '#303133',
					fontWeight: 'blod',
					transform: 'scale(1.08)'
				},
				inactiveStyle: {
					color: '#999999',
					transform: 'scale(1)'
				},
			}
		},
		onLoad() {

		},
		methods: {
			clickNav(e) {
				// this.loadState = true
				// this.loadMoreState = 'more'
				// this.blogList = []
				this.navAction = e.index
				// this.getData()
			},
		}
	}
</script>

<style>
	.content {
		
	}

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
