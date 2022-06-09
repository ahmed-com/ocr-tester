<template>
	<v-img id="img-element" v-bind="$attrs">
		<v-app-bar flat color="rgba(0, 0, 0, 0)">
			<v-spacer></v-spacer>

			<v-btn color="red" icon @click="$emit('close')">
				<v-icon>mdi-close</v-icon>
			</v-btn>
		</v-app-bar>
	</v-img>
</template>

<script>
	export default {
        data: ()=>({
            imgElement: null,
            resizeObserver: null,
        }),

        methods: {
            emitSize(){
                this.$emit("size", {
                    imgH: this.imgElement.offsetHeight,
                    imgW: this.imgElement.offsetWidth,
                })
            }
        },

        mounted(){
            this.imgElement = document.getElementById('img-element')
            this.resizeObserver = new ResizeObserver(()=> this.emitSize()).observe(this.imgElement)
        },

        destroy(){
            this.resizeObserver.disconnect();
        }
    };
</script>

<style></style>
