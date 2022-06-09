<template>
	<v-container>
		<v-row class="page-height mt-6">
			<v-col cols="5" class="grey lighten-2 fill-height center-items">
				<image-output
					v-if="imgURL"
					:src="imgURL"
					@close="onImageClose"
					@size="measureSize"
				></image-output>
				<image-input v-else @change="onImageSelected"></image-input>
			</v-col>
			<v-spacer></v-spacer>
			<v-col cols="5" class="grey lighten-2 fill-height center-items">
				<text-output
					:imgURL="imgURL"
					:imgH="imgH"
					:imgW="imgW"
				></text-output>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
	import ImageInput from './ImageInput.vue';
	import ImageOutput from './ImageOutput.vue';
	import TextOutput from './TextOutput.vue';
	export default {
		components: { ImageInput, ImageOutput, TextOutput },

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

			measureSize({imgH, imgW}) {
				this.imgH = imgH
				this.imgW = imgW
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

<style scoped>
	.page-height {
		height: 70vh;
	}

	.center-items {
		display: grid;
		place-items: center;
	}
</style>
