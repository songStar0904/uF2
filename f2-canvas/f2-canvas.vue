<template>
	<view class="f2-canvas">
		<canvas class="f2-canvas" :canvas-id="canvasId" :id="canvasId" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd"
		 @longtap="press" v-show="showCanvas">
		</canvas>
		<image class="f2-canvas" :src="temImage" mode="" v-show="!showCanvas"></image>
	</view>
</template>

<script>
	// f2-canvas.js
	import Renderer from './lib/renderer';
	import F2 from './lib/f2';
	// #ifdef  MP-WEIXIN
	// 适配小程序的事件机制
	F2.Util.addEventListener = function(source, type, listener) {
		source.addListener && source.addListener(type, listener);
	};

	F2.Util.removeEventListener = function(source, type, listener) {
		source.removeListener && source.removeListener(type, listener);
	};

	F2.Util.createEvent = function(event, chart) {
		const type = event.type;
		let x = 0;
		let y = 0;
		const touches = event.touches;
		if (touches && touches.length > 0) {
			x = touches[0].x;
			y = touches[0].y;
		}

		return {
			type,
			chart,
			x,
			y
		};
	};
	// #endif
	export default {
		props: {
			canvasId: {
				type: String,
				value: 'f2-canvas'
			},
			opts: {
				type: Object
			}
		},
		data() {
			return {
				showCanvas: true,
				temImage: undefined
			}
		},
		mounted() {
			if (!this.opts) {
				console.warn('组件需绑定 opts 变量，例：<ff-canvas id="mychart-dom-bar" ' +
					'canvas-id="mychart-bar" opts="{{ opts }}"></ff-canvas>');
				return;
			}

			if (!this.opts.lazyLoad && this.opts.onInit) {
				this.init();
			}
		},
		methods: {
			init(callback) {
				// #ifdef  MP-WEIXIN
					const version = wx.version.version.split('.').map(n => parseInt(n, 10));
					const isValid = version[0] > 1 || (version[0] === 1 && version[1] > 9) ||
						(version[0] === 1 && version[1] === 9 && version[2] >= 91);
					if (!isValid) {
						console.error('微信基础库版本过低，需大于等于 1.9.91。');
						return;
					}
				// #endif
				const ctx = wx.createCanvasContext(this.canvasId, this); // 获取小程序上下文
				const canvas = new Renderer(ctx);
				this.canvas = canvas;

				this.f2 = wx.createSelectorQuery().in(this).select('.f2-canvas')
				this.f2.boundingClientRect(res => {
					if (typeof callback === 'function') {
						this.chart = callback(canvas, res.width, res.height);
					} else if (this.opts && this.opts.onInit) {
						this.chart = this.opts.onInit(canvas, res.width, res.height);
					}
				}).exec();
			},
			touchStart(e) {
				console.log(this.canvas, e)
				if (this.canvas) {
					this.canvas.emitEvent('touchstart', [e]);
				}
			},
			touchMove(e) {
				if (this.canvas) {
					this.canvas.emitEvent('touchmove', [e]);
				}
			},
			touchEnd(e) {
				if (this.canvas) {
					this.canvas.emitEvent('touchend', [e]);
				}
			},
			press(e) {
				if (this.canvas) {
					this.canvas.emitEvent('press', [e]);
				}
			},
			closeCanvas() {
				return new Promise((resolve, reject) => {
					setTimeout(() => {
						wx.canvasToTempFilePath({
						  canvasId: this.canvasId,
						  success: res => {
						    console.log(res.tempFilePath)
							this.showCanvas = false
							this.temImage = res.tempFilePath
							resolve()
						  },
						  fail: () => {
						  	reject()
						  }
						}, this)
					}, 500)
				})
			},
			openCanvas() {
				this.showCanvas = true
			}
		}
	}
</script>

<style>
	.f2-canvas {
		width: 100%;
		height: 100%;
	}
</style>
