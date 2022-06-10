<template>
		<v-row class="page-height mt-6">
			<v-col cols="3" class="grey lighten-2 fill-height center-items">
				<image-output
					v-if="imgURL"
					:src="imgURL"
					@close="onImageClose"
					@size="measureSize"
				></image-output>
				<image-input v-else @change="onImageSelected"></image-input>
			</v-col>
			<v-spacer></v-spacer>
			<v-col cols="3" class="grey lighten-2 fill-height center-items">
				<preprocessed
                    :imgURL="imgURL"
                    :imgH="imgH"
                    :imgW="imgW"
                    @drawing="setCanvas"
                ></preprocessed>
			</v-col>
			<v-spacer></v-spacer>
			<v-col cols="3" class="grey lighten-2 fill-height center-items">
                <text-output-2
                    :canvas="canvas"
                ></text-output-2>
				<text-output
                v-if="false"
					:imgURL="imgURL"
					:imgH="imgH"
					:imgW="imgW"
				></text-output>
			</v-col>
		</v-row>
</template>

<script>
	import ImageInput from './ImageInput.vue';
	import ImageOutput from './ImageOutput.vue';
	import Preprocessed from './Preprocessed.vue';
	import TextOutput from './TextOutput.vue';
import TextOutput2 from './TextOutput2.vue';
	export default {
		components: { ImageInput, ImageOutput, TextOutput, Preprocessed, TextOutput2 },

		data: () => ({
			imgInputElement: null,
            canvas: null,
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

            setCanvas(c){
                this.canvas = c;
            },

			measureSize({ imgH, imgW }) {
				this.imgH = imgH;
				this.imgW = imgW;
			},

			onImageClose() {
				this.imgURL = null;
                this.canvas = null;
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
