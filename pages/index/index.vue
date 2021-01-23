<template>
	<view class="page">
	  <view v-for="(chart) in others" :key="chart.name" 
	    @tap="gotoPage(chart)"
	    class="list-item-block">
	    <icon type="success_no_circle" size="20"/>
	    {{chart.value}}
	  </view>
	  <view  v-for="chart in charts" :key="chart.name"  class="list-item">
	     <image :src="`/static/${chart.name}.png`" mode="aspectFit" @tap="gotoPage(chart)" ></image> 
	    {{chart.value}}
	  </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				charts: [
				    { name: 'radial-bar', value: 'iwatch健康记录' },      
				    { name: 'line', value: '折线图' },
				    { name: 'area', value: '区域图' },
				    { name: 'column', value: '柱状图' },
				    { name: 'bar', value: '条形图' },
				    { name: 'dodge', value: '分组柱状图' },
				    { name: 'stackBar', value: '层叠条形图' },
				    { name: 'ring', value: '环图' },
				    { name: 'pie', value: '饼图' },
				    { name: 'rose', value: '玫瑰图' },
				    { name: 'radar', value: '雷达图' },
				    { name: 'gauge', value: '仪表盘' },
				    { name: 'double-axis', value: '双 Y 轴' },
				    { name: 'k', value: 'K 线图(异步获取数据)' },
				    { name: 'stack-area', value: '层叠区域图' },
				    { name: 'multiCharts', value: '多图表好性能' }
				  ],
				  others: [
				    { name: 'scroll-line', value: '线图平移交互(长按展示 tooltip)' },
				    { name: 'steps-pan', value: '每日步数（柱状图平移）' },
				    { name: 'pie-select', value: '饼图选中交互' },
				    { name: 'column-select', value: '柱状图选中交互(可取消选中)' },
				    { name: 'gradient-column', value: '渐变色柱状图' }
				  ]
				}
		},
		methods: {
			gotoPage({name: page, value: title}) {
			  uni.navigateTo({
			    url: '/pages/charts/' + page + '/' + page,
				success() {
					uni.setNavigationBarTitle({
						title
					})
				}
			  })
			}
		}
	}
</script>

<style>
	.page {
	  padding: 10px;
	  margin: 0;
	  list-style: none;
	  
	  display: -webkit-box;
	  display: -moz-box;
	  display: -ms-flexbox;
	  display: -webkit-flex;
	  display: flex;
	  
	  -webkit-flex-flow: row wrap;
	  justify-content: space-between;
	  font-family: -apple-system-font, "Helvetica Neue", sans-serif;
	}
	.list-item {
	  padding: 5px;
	  width: 70px;
	  height: 100px;
	  text-align: center;
	  font-size: 12px;
	  margin-top: 10px;
	}
	
	.list-item image {
	  box-sizing: border-box;
	  width: 100%;
	  height: 70px; 
	  padding: 12px;
	  background-color: #fff;
	  border: 1px solid #eee;
	  border-radius: 4px;  
	  display: block;
	  margin: 0 auto;
	  margin-bottom: 10px;
	}
	
	.list-item-block {
	  height: 80rpx;
	  line-height: 80rpx;
	  /* text-indent: 30rpx; */
	  border-bottom: 2rpx solid #cccccc; 
	  width: 100%;
	  color: #808080;
	  font-size: 12px;
	}
	
</style>
