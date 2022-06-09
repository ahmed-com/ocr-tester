<template>
	<div class="display-grid page-height ma-6">
		<div class="grey lighten-2 fill-height center-items same-position">
			<image-output
				v-if="imgURL"
				:src="imgURL"
				@close="onImageClose"
				@size="measureSize"
			></image-output>
		</div>
		<div class="transparent fill-height center-items same-position">
			<image-input v-if="!imgUrl" @change="onImageSelected"></image-input>
			<text-output
				:imgURL="imgURL"
				:imgH="imgH"
				:imgW="imgW"
			></text-output>
		</div>
	</div>
</template>

<script>
	import ImageInput from './ImageInput.vue';
	import ImageOutput from './ImageOutput.vue';
	import TextOutput from './TextOutput.vue';
	export default {
		components: { ImageOutput, ImageInput, TextOutput },

		data: () => ({
			imgInputElement: null,
			imgURL: null,
			imgW: 0,
			imgH: 0,
		}),

		methods: {
			onImageSelected(ev) {
				this.imgInputElement = ev.target;
				const imgFile = ev.target.files[0];

				const reader = new FileReader();
				reader.addEventListener(
					'load',
					() => (this.imgURL = reader.result),
					false
				);

				reader.readAsDataURL(imgFile);
			},

			measureSize({ imgH, imgW }) {
				this.imgH = imgH;
				this.imgW = imgW;
			},

			onImageClose() {
				this.imgURL = null;
				this.imgH = 0;
				this.imgW = 0;
				this.imgInputElement.value = null;
				this.imgInputElement = null;
			},
		},
	};
</script>
, TextOutput

<style>
	.display-grid {
		display: grid;
	}

	.same-position {
		grid-row: 1;
		grid-column: 1;
	}

	.page-height {
		height: 70vh;
	}

	.center-items {
		display: grid;
		place-items: center;
	}

	.transparent {
		background-color: transparent;
        z-index: 10;
	}
</style>
