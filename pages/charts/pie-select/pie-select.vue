<template>
	<view class="container">
	  <text>{{ message }}</text>  
	  <ff-canvas id="pieSelect" canvas-id="pieSelect" :opts="opts" ref="f2"></ff-canvas>
	</view>
</template>

<script>
	import F2 from '../../../f2-canvas/lib/f2';
	
	let chart;
	export default {
		data() {
			return {
				opts: {
				  lazyLoad: true // 延迟加载组件
				},
				message: '请点击饼图，观察我的变化'
			}
		},
		mounted() {
			this.$refs.f2.init(this.initChart)
		},
		methods: {
			initChart(canvas, width, height) {
			  var data = [
			    { name: '芳华', percent: 0.4, a: '1' },
			    { name: '妖猫传', percent: 0.2, a: '1' },
			    { name: '机器之血', percent: 0.18, a: '1' },
			    { name: '心理罪', percent: 0.15, a: '1' },
			    { name: '寻梦环游记', percent: 0.05, a: '1' },
			    { name: '其他', percent: 0.12, a: '1' }
			  ];
			  var chart = new F2.Chart({
			    el: canvas,
			    width,
			    height
			  });
			  chart.source(data, {
			    percent: {
			      formatter: function formatter(val) {
			        return val * 100 + '%';
			      }
			    }
			  });
			  chart.legend({
			    position: 'right'
			  });
			  chart.tooltip(false);
			  chart.coord('polar', {
			    transposed: true,
			    radius: 0.85,
			    innerRadius: 0.618
			  });
			  chart.axis(false);
			  chart
			    .interval()
			    .position('a*percent')
			    .color('name', ['#1890FF', '#13C2C2', '#2FC25B', '#FACC14', '#F04864', '#8543E0'])
			    .adjust('stack')
			    .style({
			      lineWidth: 1,
			      stroke: '#fff',
			      lineJoin: 'round',
			      lineCap: 'round'
			    });
			  			
			  chart.interaction('pie-select', {
			    cancelable: false, // 不允许取消选中
			    animate: {
			      duration: 300,
			      easing: 'backOut'
			    },
			    onEnd:(ev) => {
			      const { shape, data, shapeInfo, selected } = ev;
			      if (shape) {
			        if (selected) {
			  			this.message = data.name + ': ' + data.percent * 100 + '%'
			        }
			      }
			    }
			  });
			  			
			  chart.render();
			  return chart;
			}
		}
	}
</script>

<style>
	text {
	  position: absolute;
	  top: 80rpx;
	}
</style>
