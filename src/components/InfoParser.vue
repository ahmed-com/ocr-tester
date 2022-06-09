<template>
	<div class="hocr-container">
        <div
            v-for="word in info.data.words"
            :key="`x0: ${word.bbox.x0}, y0: ${word.bbox.y0}`"
            class="ocrx-word"
            :class="{
                bold: word.is_bold,
                italic: word.is_italic,
                underline: word.is_underlined
            }"
            :style="{
                position: 'fixed',
                display: 'inline-block',
                fontSize: `${word.font_size}`,
                left: `${word.bbox.x0}px`,
                top: `${word.bbox.y0}px`,
                height: `${word.bbox.y1 - word.bbox.y0}px`,
                width: `${word.bbox.x1 - word.bbox.x0}px`,
            }"
        >
            {{word.text}}
        </div>
    </div>
</template>

<script>
	export default {
		props: ['info'],
	};
</script>

<style scoped>
	.hocr-container {
		transform: perspective(0);
	}

    .bold {
        font-weight: bold;
    }

    .italic {
        font-style: italic;
    }

    .underline{
        text-decoration: underline;
    }
</style>
