<template>
	<div id="hocr-host"></div>
</template>

<script>
	export default {
		props: ['hocr'],

		mounted() {
			this.renderHocr();
		},

		watch: {
			hocr() {
				this.renderHocr();
			},
		},

		methods: {
			renderHocr() {
				const container = document.createElement('div');
				container.setAttribute('style', 'transform: perspective(0);');
				container.innerHTML = this.hocr;
				container.querySelectorAll('span.ocrx_word').forEach((span) => {
					const title = span.getAttribute('title');
					const bbox = title
						.split(';')[0]
						.split(' ')
						.splice(1, 4)
					const styleTxt = `left: ${bbox[0]}px; top: ${bbox[1]}px;  position: fixed;`;
					span.setAttribute('style', styleTxt);
				});
				const host = document.getElementById('hocr-host');
				if (host.lastChild) host.removeChild(host.lastChild);
				host.appendChild(container);
			},
		},
	};
</script>

<style scoped>
	.hocr-container {
		transform: perspective(0);
	}
</style>
