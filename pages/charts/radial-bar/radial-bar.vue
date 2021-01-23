<template>
	<view class="container">
	  <ff-canvas id="radialBar-dom" canvas-id="radialBar" :opts="opts" ref="f2"></ff-canvas>
	</view>
</template>

<script>
	import F2 from '../../../f2-canvas/lib/f2';
	const { Shape, Util, Global, G, Animate } = F2;
	const { Vector2 } = G;
	function initChart(canvas, width, height) {
	  // customize shape and animation
	  Shape.registerShape('interval', 'tick', {
	    draw(cfg, container) {
	      const points = this.parsePoints(cfg.points);
	      const style = Util.mix({
	        stroke: cfg.color
	      }, Global.shape.interval, cfg.style);
	      if (cfg.isInCircle) {
	        let newPoints = points.slice(0);
	        if (this._coord.transposed) {
	          newPoints = [points[0], points[3], points[2], points[1]];
	        }
	
	        const { x, y } = cfg.center;
	        const v = [1, 0];
	        const v0 = [newPoints[0].x - x, newPoints[0].y - y];
	        const v1 = [newPoints[1].x - x, newPoints[1].y - y];
	        const v2 = [newPoints[2].x - x, newPoints[2].y - y];
	
	        let startAngle = Vector2.angleTo(v, v1);
	        let endAngle = Vector2.angleTo(v, v2);
	        const r0 = Vector2.length(v0);
	        const r = Vector2.length(v1);
	
	        if (startAngle >= 1.5 * Math.PI) {
	          startAngle = startAngle - 2 * Math.PI;
	        }
	
	        if (endAngle >= 1.5 * Math.PI) {
	          endAngle = endAngle - 2 * Math.PI;
	        }
	
	        const coord = this._coord;
	
	        const lineWidth = r - r0;
	        const newRadius = r - (lineWidth / 2);
	
	        return container.addShape('Arc', {
	          className: 'interval',
	          attrs: Util.mix({
	            x,
	            y,
	            startAngle,
	            endAngle,
	            r: newRadius,
	            lineWidth,
	            lineCap: 'round',
	            shadowColor: "rgba(0, 0, 0, 0.6)",
	            shadowOffsetX: 0,
	            shadowOffsetY: -5,
	            shadowBlur: 50
	          }, style)
	        });
	      }
	    }
	  });
	
	  Animate.registerAnimation('waveIn', function (shape, animateCfg, coord) {
	    const startAngle = shape.attr('startAngle');
	    const endAngle = shape.attr('endAngle');
	    shape.attr('endAngle', startAngle);
	    shape.animate().to(Util.mix({
	      attrs: {
	        endAngle: endAngle
	      }
	    }, animateCfg));
	  });
	  // ------
	
	  const data = [
	    { name: 'activity1', percent: 2370, color: '#1ad5de', icon: 'stand.png', bgColor: '#183C3D' },
	    { name: 'activity2', percent: 80, color: '#a0ff03', icon: 'walk.png', bgColor: '#324214' },
	    { name: 'activity3', percent: 65, color: '#e90b3a', icon: 'run.png', bgColor: '#40131D' }
	  ];
	
	  const chart = new F2.Chart({
	    el: canvas,
	    width,
	    height
	  });
	
	  chart.source(data, {
	    percent: {
	      max: 100
	    }
	  });
	  chart.legend(false);
	  chart.coord('polar', {
	    transposed: true,
	    innerRadius: 0.382
	  });
	  chart.axis(false);
	  chart.interval()
	    .position('name*percent')
	    .color('color', val => {
	      return val;
	    })
	    .shape('tick')
	    .size(26)
	    .animate({
	      appear: {
	        animation: 'waveIn',
	        duration: 1500,
	        easing: 'elasticOut'
	      },
	      update: {
	        duration: 1500,
	        easing: 'elasticOut'
	      }
	    });
	
	  data.map((obj, index) => {
	    // background
	    chart.guide().arc({
	      start: [obj.name, 0],
	      end: [obj.name, 99.98],
	      top: false,
	      style: {
	        lineWidth: 26,
	        stroke: obj.bgColor
	      }
	    });
	  });
	  chart.render();
	/*
	  const updateData = function () {
	    for (let i = 0; i < data.length; ++i) {
	      data[i].percent = Math.floor((Math.random() * 60) + 20);
	    }
	    chart.changeData(data);
	    setTimeout(updateData, 1500);
	  };
	
	  setTimeout(updateData, 1500); */   
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
	.container {
	    margin: 0 auto;
	    background-color: #151515;
	    border-radius: 5px;
	    box-shadow: 0px 0px 1px 0px #06060d;
	    padding: 50px;
	}
</style>
