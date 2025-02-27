<template>
	<div class="import-form">
		<el-dialog v-model="dialogVisible" title="import blocks" width="600" :close-on-click-modal="false">
			<el-form :model="blocksData" label-width="auto" style="max-width: 600px">
				<el-form-item label="category: ">
					<el-input v-model="blocksData.category" />
				</el-form-item>
				<template v-for="(block, i) in blocksData.blocks" :key="i">
					<el-row>
						<el-col :span="4">
							blocks{{ i + 1 }}:
						</el-col>
						<el-col :span="20">
							<el-form-item label="opcode: ">
								<el-input v-model="block.opcode" />
							</el-form-item>
							<el-form-item label="message: ">
								<el-input v-model="block.message" />
							</el-form-item>
							<el-row>
								<el-col :span="4">参数列表</el-col>
								<el-col :span="2">
									<el-icon :size="24" @click="addArg(block)">
										<CirclePlus />
									</el-icon>
								</el-col>
								<el-col :span="2">
									<el-icon :size="24" @click="minArg(block)">
										<Minus />
									</el-icon>
								</el-col>
							</el-row>
							<template v-for="(arg, i) in block.argList" :key="i">
								<el-row>
									<el-col :span="8">
										<el-form-item label="name: ">
											<el-input v-model="arg.argName" />
										</el-form-item>
									</el-col>
									<el-col :span="8">
										<el-form-item label="type: ">
											<el-input v-model="arg.argType" />
										</el-form-item>
									</el-col>
									<el-col :span="8">
										<el-form-item label="value: ">
											<el-input v-model="arg.default" />
										</el-form-item>
									</el-col>
								</el-row>
							</template>
						</el-col>
					</el-row>
					<p></p>
				</template>
				<p></p>
				<el-row>
					<el-col :span="6">
						<el-button type="primary" plain @click="addBlock">add block</el-button>
					</el-col>
					<el-col :span="6">
						<el-button type="primary" plain @click="delBlock">delete block</el-button>
					</el-col>
				</el-row>
			</el-form>
			<template #footer>
				<div class="dialog-footer">
					<el-button type="primary" @click="onSubmit">
						import
					</el-button>
				</div>
			</template>
		</el-dialog>
	</div>
</template>
<script setup>
import { ref, reactive, defineEmits } from 'vue'
const emit = defineEmits(['post-data'])

// do not use same name with ref
const blocksData = reactive({
	category: 'Robot',
	blocks: [{
		opcode: 'robot_test',
		message: 'block test with text field %1 and number field %2',
		argList: [
			{ argName: 'TEXT', argType: 'text', default: 'hello' },
			{ argName: 'NUM', argType: 'math_number', default: 1 },
		]
	},
	{
		opcode: 'robot_arg1',
		message: 'block test with text field %1 ',
		argList: [
			{ argName: 'TEXT', argType: 'text', default: 'hello world' }
		]
	}]
})

const onSubmit = () => {
	emit('post-data', blocksData)
	dialogVisible.value = false
}

const dialogVisible = ref(true)

const addArg = (block) => {
	block.argList.push({ argName: '', argType: '', default: '' })
}
const minArg = (block) => {
	if (block.argList.length) block.argList.pop()
}
const addBlock = () => {
	blocksData.blocks.push({
		opcode: '',
		message: '',
		argList: []
	})
}
const delBlock = () => {
	if (blocksData.blocks.length) blocksData.blocks.pop()
}
</script>

<style scoped>
.import-form {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
</style>
