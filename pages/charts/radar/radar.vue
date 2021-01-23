<template>
	<view class="container">
	  <ff-canvas id="radar-dom" canvas-id="radar" :opts="opts" ref="f2"></ff-canvas>
	</view>
</template>

<script>
	import F2 from '../../../f2-canvas/lib/f2';
	
	let chart = null;
	
	function initChart(canvas, width, height) {
	  const data = [
	    { name: '超大盘能力', value: 6.5 },
	    { name: '抗跌能力', value: 9.5 },
	    { name: '稳定能力', value: 9 },
	    { name: '绝对收益能力', value: 6 },
	    { name: '选证择时能力', value: 6 },
	    { name: '风险回报能力', value: 8 }
	  ];
	
	  const chart = new F2.Chart({
	    el: canvas,
	    width,
	    height
	  });
	
	  chart.source(data, {
	    value: {
	      min: 0,
	      max: 10
	    }
	  });
	  chart.coord('polar');
	  chart.tooltip(false); // 关闭 tooltip
	  chart.axis('value', {
	    grid: {
	      lineDash: null
	    },
	    label: null,
	    line: null
	  });
	  chart.axis('name', {
	    grid: {
	      lineDash: null
	    }
	  });
	  chart.area()
	    .position('name*value')
	    .color('#FE5C5B')
	    .style({
	      fillOpacity: 0.2
	    })
	    .animate({
	      appear: {
	        animation: 'groupWaveIn'
	      }
	    });
	  chart.line()
	    .position('name*value')
	    .color('#FE5C5B')
	    .size(1)
	    .animate({
	      appear: {
	        animation: 'groupWaveIn'
	      }
	    });
	  chart.point().position('name*value').color('#FE5C5B').animate({
	    appear: {
	      delay: 300
	    }
	  });
	
	  chart.guide().text({
	    position: ['50%', '50%'],
	    content: '73',
	    style: {
	      fontSize: 32,
	      fontWeight: 'bold',
	      fill: '#FE5C5B'
	    }
	  });
	  chart.render();
	  return chart;
	}
	export default {
		data() {
			return {
				opts: {
				}
			}
		},
		mounted() {
			this.$refs.f2.init(initChart)
		}
	}
</script>

<style>
</style>
