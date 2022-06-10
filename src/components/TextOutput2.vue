<template>
	<div>
		<v-progress-circular
			indeterminate
			color="primary"
			v-if="isLoading"
		></v-progress-circular>
		<div v-else>
			<v-sheet
				class="transparent-background"
				v-if="info"
				:height="imgH"
				:width="imgW"
			>
				<hocr-parser v-if="false" :hocr="info.data.hocr"></hocr-parser>
				<info-parser v-if="false" :info="info"></info-parser>
				<info-parser-2 v-if="true" :info="info"></info-parser-2>
			</v-sheet>
			<h2 v-else class="text--grey">Ready to parse</h2>
		</div>
	</div>
</template>

<script>
	import { createWorker, createScheduler } from 'tesseract.js';
	import HocrParser from './HocrParser.vue';
	import InfoParser from './InfoParser.vue';
	import InfoParser2 from './InfoParser2.vue';

	export default {
		components: { HocrParser, InfoParser, InfoParser2 },
		props: ['canvas'],

		data: () => ({
			isLoading: true,
			scheduler: createScheduler(),
			info: null,
		}),

        computed: {
            imgH(){
                return this.canvas.height;
            },
            
            imgW(){
                return this.canvas.width;
            }
        },

		methods: {
			async recognize() {
				const info = await this.scheduler.addJob('recognize', this.canvas);
				console.info(info);
				this.info = info;
			},
		},

		async mounted() {
			for (let i = 0; i < 4; i++) {
				const worker = createWorker();
				await worker.load();
				await worker.loadLanguage('eng');
				await worker.initialize('eng');
				this.scheduler.addWorker(worker);
			}
			this.isLoading = false;
		},

		watch: {
			// async imgURL() {
			// 	this.isLoading = true;
			// 	await this.recognize();
			// 	this.isLoading = false;
			// },
			// async imgW() {
			// 	this.isLoading = true;
			// 	await this.recognize();
			// 	this.isLoading = false;
			// },
			async canvas() {
                if(!this.canvas){
                    this.isLoading = false;
                    this.info = null;
                    return;
                }
				this.isLoading = true;
				await this.recognize();
				this.isLoading = false;
			},
		},
	};
</script>

<style scoped>
	.transparent-background {
		background-color: transparent;
	}
</style>
