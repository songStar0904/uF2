<template>
	<view class="container">
	  <ff-canvas id="bar-dom" canvas-id="bar" :opts="opts" ref="f2"></ff-canvas>
	</view>
</template>

<script>
	import F2 from '../../../f2-canvas/lib/f2';
	
	let chart = null;
	
	function initChart(canvas, width, height) {
	  chart = new F2.Chart({
	    el: canvas,
	    width,
	    height
	  });
	
	  var Global = F2.Global;
	  var data = [
	    { country: '巴西', population: 18203 }, 
	    { country: '印尼', population: 23489 },
	    { country: '美国', population: 29034 }, 
	    { country: '印度', population: 104970 }, 
	    { country: '中国', population: 131744 }
	  ];
	
	  chart.source(data, {
	    population: {
	      tickCount: 5
	    }
	  });
	  chart.coord({
	    transposed: true
	  });
	  chart.axis('country', {
	    line: Global._defaultAxis.line,
	    grid: null
	  });
	  chart.axis('population', {
	    line: null,
	    grid: Global._defaultAxis.grid,
	    label: function label(text, index, total) {
	      var textCfg = {};
	      if (index === 0) {
	        textCfg.textAlign = 'left';
	      } else if (index === total - 1) {
	        textCfg.textAlign = 'right';
	      }
	      return textCfg;
	    }
	  });
	  chart.interval().position('country*population');
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
