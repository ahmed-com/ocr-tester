<template>
	<canvas id="p-canvas" :width="imgW" :height="imgH"></canvas>
</template>

<script>
	import { range } from '../utils';

	const threshold = 180;

	export default {
		props: ['imgURL', 'imgW', 'imgH'],

		methods: {
			async drawImage() {
				const img = await this.getImg(
					this.imgURL,
					this.imgW,
					this.imgH
				);

				const canvas = document.getElementById('p-canvas');
				const ctx = canvas.getContext('2d');
				const canvas_temp = document.createElement('canvas');
				canvas_temp.setAttribute('width', this.imgW);
				canvas_temp.setAttribute('height', this.imgH);
				const ctx_temp = canvas_temp.getContext('2d');
				ctx_temp.drawImage(img, 0, 0, this.imgW, this.imgH);
				const frame = ctx_temp.getImageData(0, 0, this.imgW, this.imgH);

				const pixelCount = frame.data.length / 4;

				const grayArray = range(pixelCount)
					.map((i) => ({
						r: frame.data[i * 4 + 0],
						g: frame.data[i * 4 + 1],
						b: frame.data[i * 4 + 2],
					}))
					.map(this.RGBtoGray);

				const pixelSum = grayArray.reduce((x, y) => x + y, 0);
				const average = pixelSum / pixelCount;

				const darkModeFactor = average > 128 ? 0 : 255;

				grayArray
					.map((g) => this.grayToBin(g, darkModeFactor))
					.forEach((bin, i) => {
						frame.data[i * 4 + 0] = bin;
						frame.data[i * 4 + 1] = bin;
						frame.data[i * 4 + 2] = bin;
					});

				ctx.putImageData(frame, 0, 0);
				this.$emit('drawing', canvas);
			},

			async getImg(imgURL, imgW, imgH) {
				return new Promise((resolve) => {
					const img = new Image(imgW, imgH);
					img.onload = () => resolve(img);
					img.src = imgURL;
				});
			},

			RGBtoGray: ({ r, g, b }) => 0.299 * r + 0.587 * g + 0.114 * b,
			grayToBin: (g , f) => (g > threshold ? (255 - f) : f),
		},

		watch: {
			async imgH() {
				await this.drawImage();
			},
		},

		async mounted() {
			await this.drawImage();
		},
	};
</script>

<style></style>
