<template>
	<div class="container">
		<import-form @post-data="loadBlockly"/>
		<div class="blockly" id="blocklyDiv"></div>
		<code-box />
	</div>
</template>
<script>
/* eslint-disable */
import CodeBox from "./CodeBox.vue"
import ImportForm from "./ImportForm.vue"
import { defineComponent, onMounted, ref } from 'vue'
let Blockly = window.Blockly
let workspace = null

export default defineComponent({
	components: { CodeBox, ImportForm },
	setup() {
		const blocksInit = (blocksData) => {
			for (const block of blocksData.blocks) {
				Blockly.Blocks[block.opcode] = {
					init: function () {
						console.log(block.message)
						this.jsonInit({
							// "message0": "robot play audio %1",
							"message0": block.message,
							"args0": block.argList.map((arg) => {
								return {
									"type": 'input_value',
									"name": arg.argName
								}
							}),
							"category": Blockly.Categories.motion,
							"extensions": ["colours_motion", "shape_statement"]
						});
					}
				}
				Blockly.Blocks[block.opcode].callback = () => {
					console.log('do something')
				}
			}
		}
		const getXml = (blocksData) => {
			const preXml = `
				<xml style="display: none">
					<category name="%{BKY_CATEGORY_EVENTS}" id="events" colour="#FFD500" secondaryColour="#CC9900">
						<block type="event_whenflagclicked"></block>
					</category>
					<sep gap="36"/>		
			`
			let mainXml = `<category name="${blocksData.category}" id="${blocksData.category}" colour="#9966FF" secondaryColour="#774DCB">`
			for (const block of blocksData.blocks) {
				let argXml = ''
				for (const arg of block.argList) {
					argXml = argXml + `
					<value name="${arg.argName}">
						<shadow type="${arg.argType}">
							<field name="${arg.argType ==='text'?'TEXT':'NUM'}">${arg.default}</field>
						</shadow>
					</value>
				`
				}
				mainXml += `
				<block type="${block.opcode}">
					${argXml}
				</block>
			`
			}
			mainXml += `</category>
			<sep gap="36"/>`

			const endXml = `</xml>`

			const allXml = preXml + mainXml + endXml
			return allXml
		}

		const genLuaCode = () => {
			console.log(workspace)
			let code = Blockly.Lua.genCode(workspace)
			console.log(code)
			// createCodeNode(code)
			// window.hljs.highlightAll();
			// window.hljs.initLineNumbersOnLoad();
		}
		const loadBlockly = (blocksData)=>{
			// const blocksData = {
			// 	"blockType": "robot",
			// 	"blocks": [
			// 		{
			// 			"opcode": "robot_run",
			// 			"message": "robot run with speed %1 and rotation %2",
			// 			"argList": [
			// 				{
			// 					"argType": "number",
			// 					"argName": "SPEED"
			// 				},
			// 				{
			// 					"argType": "number",
			// 					"argName": "ROTATION"
			// 				}
			// 			]
			// 		},
			// 		{
			// 			"opcode": "robot_play",
			// 			"message": "robot play audio %1",
			// 			"argList": [
			// 				{
			// 					"argType": "number",
			// 					"argName": "TEXT1"
			// 				}
			// 			]
			// 		}
			// 	]
			// }
			console.log(blocksData)
			blocksInit(blocksData)
			workspace = Blockly.inject('blocklyDiv', {
				comments: true,
				disable: false,
				collapse: false,
				media: './media/',
				readOnly: false,
				rtl: false,
				scrollbars: true,
				toolbox: getXml(blocksData),
				grid:
				{
					spacing: 20,
					length: 3,
					colour: '#ccc',
					snap: true
				},
				toolboxPosition: 'start',
				horizontalLayout: false,
				trashcan: true,
				sounds: false,
				zoom: {
					controls: true,
					wheel: true,
					startScale: 0.675,
					maxScale: 4,
					minScale: 0.25,
					scaleSpeed: 1.1
				},
				colours: {
					fieldShadow: 'rgba(255, 255, 255, 0.3)',
					dragShadowOpacity: 0.6
				}
			});
			console.log(workspace)
			//监听workspace的变化
			workspace.addChangeListener((e) => {
				// console.log(e)
				if (e.element === "stackclick") {
					// console.log(workspace.blockDB_[e.blockId]?.callback())
				}
				if ((e.type == "move" || e.type == "ui")) {
					// genLuaCode()  //todo
				}
			});

		}

		onMounted(() => {
			
		})
		return {
			loadBlockly
		}
	},
})
</script>

<style scoped>
.container {
	position: fixed;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
}

.blockly {
	width: 70%;
	height: 100%;
	float: left;
}

.blocklyToolboxDiv {
	border-right: 1px solid #ddd;
	border: 1px solid #ccc;
}
::v-deep.blocklyFlyout{
	background: #ccc;
}

.blocklyFlyout {
	background-color: #d1d1d1;
	border-right: 1px solid #ddd;
}

/* todo  scrollbar details  */
::v-deep .blocklyScrollbarVertical {
	width: 1px;
	height: 8px;
}
</style>
