<template>
	<label
		for="real-input"
		class="rounded-lg primary pa-2 white--text elevation-4 cursor-pointer"
	>
		Upload Image
		<input v-on="$listeners" type="file" id="real-input" v-show="false" />
	</label>
</template>

<script>
	export default {};
</script>

<style scoped>
	.cursor-pointer {
		cursor: pointer;
	}
</style>
