<template>
	<div class="code-box" >
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
	text-align: left;
	background: #f6f6f6;
	box-sizing: border-box;
    border-left: 1px solid #eee;
}
</style>
