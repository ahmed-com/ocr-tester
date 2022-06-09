<template>
	<svg class="hocr-container">
		<text
			v-for="word in info.data.words"
			:key="`x0: ${word.bbox.x0}, y0: ${word.bbox.y0}`"
			class="ocrx-word"
			:class="{
				bold: word.is_bold,
				italic: word.is_italic,
				underline: word.is_underlined,
			}"
            :x="word.bbox.x0"
            :y="word.bbox.y0"
            dominant-baseline="hanging"
            :textLength="word.bbox.x1 - word.bbox.x0"
            :font-size="word.bbox.y1 - word.bbox.y0"
            lengthAdjust="spacingAndGlyphs"
            fill="transparent"
		>
            {{word.text}}
        </text>
	</svg>
</template>

<script>
	export default {
		props: ['info'],
	};
</script>

<style>
	.hocr-container {
		width: 100%;
        height: 100%;
	}

	.bold {
		font-weight: bold;
	}

	.italic {
		font-style: italic;
	}

	.underline {
		text-decoration: underline;
	}
</style>
