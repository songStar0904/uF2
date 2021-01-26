# uF2
F2 的UniApp版本，支持原生 F2 的所有功能。
注：本插件时基于[wx-f2](https://github.com/antvis/wx-f2)，所以目前仅支持微信小程序，后续会考虑支持多端。如有多端开发需求，推荐使用[uChart](https://www.ucharts.cn/)。

## 使用方法
1. 将`f2-cnvas`目录放入组件项目。
2. 引入`f2-cnvas`组件。
3. 在页面中使用：(`page`目录有相关示例)
```html
<view class="container">
  <ff-canvas id="line-dom" canvas-id="line" :opts="opts" ref="f2"></ff-canvas>
</view>
```
```js
// 引入F2
import F2 from '@/f2-canvas/lib/f2';
	
let chart = null;
// 实例化图表
function initChart(canvas, width, height) {
  // 数据
  const data = [...]
  chart = new F2.Chart({
	el: canvas,
	width,
	height
  });
  chart.source(data);
  // ...
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
```
## API
- F2 API 参见：[https://f2.antv.vision/zh/docs/api/f2](https://f2.antv.vision/zh/docs/api/f2)
