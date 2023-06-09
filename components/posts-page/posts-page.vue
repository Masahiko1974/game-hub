<template>
	<view>
		<scroll-view class="listview" style="flex: 1;" enableBackToTop="true" scroll-y @scrolltolower="loadMore()">
		  <view v-for="(item, index) in dataList" :key="item.id">
		    <posts-item :postsItem="item" @click="goDetail(item)"></posts-item>
		  </view>
		  <view class="loading-more" v-if="isLoading || dataList.length > 4">
		    <text class="loading-more-text">{{loadingText}}</text>
		  </view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name:"posts-page",
		props: {
			cateId: {
				type: [Number, String],
				default: ''
			}
		},
		data() {
			return {
				dataList: [],
			};
		},
		methods: {
			loadMore() {
				
			},
		}
	}
</script>

<style lang="scss">

</style>