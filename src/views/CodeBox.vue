<template>
	<div style="text-align: left;" >
		<h3>Lua</h3>
		<highlightjs autodetect :code="code" />
	</div>
</template>
<script>
/* eslint-disable */
import { defineComponent, onMounted, ref } from 'vue'
import 'highlight.js/lib/common';
import hljsVuePlugin from "@highlightjs/vue-plugin";

export default defineComponent({
	components: {
        highlightjs: hljsVuePlugin.component
    },
	props: {
		code: String
	},
	setup() {

		onMounted(() => {

		})
		return {

		}
	},
})
</script>

<style>
.code-box {
	width: 30%;
	height: 100%;
	float: left;
	background: #eee;
}
</style>
