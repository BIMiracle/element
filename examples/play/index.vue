<template>
	<div style="margin: 20px;">
		<el-input v-model="input" placeholder="请输入内容aaaa"></el-input>

		<el-form ref="ruleForm" :model="ruleForm" :rules="rules" label-position="right" class="demo-ruleForm">
			<el-form-item label="关联区域" prop="areaValidator">
				<el-cascader ref="areaSelect" v-model="areaValue" :props="areaProps" style="width:400px;margin-bottom:20px" />
			</el-form-item>
		</el-form>
		<el-card class="mb-10">
			<div>
				<div style="text-align:right">
					<el-button type="primary" @click="submitForm('ruleForm')">保存</el-button>
				</div>
			</div>
		</el-card>
	</div>
</template>

<script>
export default {
	data() {
		const validateArea = (rule, value, callback) => {
			let hasNode = false
      const areaArrId = []
			const arr = this.$refs['areaSelect'].getCheckedNodes()
			for (let i = 0, len = arr.length; i < len; i++) {
				const areaNode = arr[i]
				if (areaNode) {
          if (areaArrId.includes(areaNode.data.value2)) {
            callback(new Error('关联区域重复'))
            return
					}
          areaArrId.push(areaNode.data.value2)
					hasNode = true
				}
			}
			if (hasNode) {
				callback()
			} else {
				callback(new Error('请选择关联区域'))
			}
		}
		return {
			input: 'Hello Element UI!',
			areaValue: [],
			hasNode: false,
			duplicateAssociatedArea: false,
			areaProps: {
				checkStrictly: true,
				lazy: true,
				multiple: true,
				lazyLoad: this.lazyLoad,
			},
			rules: {
				areaValidator: [{ required: true, validator: validateArea, trigger: 'change' }],
			},
			ruleForm: {
				areaId: null,
				areaCode: null,
			},
			id: 0,
			id2: 1000,
			id3: 10000,
		}
	},
	methods: {
		lazyLoad(node, resolve) {
			if (node.isLeaf) {
				resolve()
				return
			}
			console.warn('加载网络')
			setTimeout(() => {
				const nodes = [
					{
						value: this.id++,
						value2: '1',
						label: '01',
						leaf: true,
					},
					{
						value: this.id2++,
						value2: '2',
						label: '02',
						leaf: false,
					},
					{
						value: this.id3++,
						value2: '3',
						label: '03',
						leaf: false,
					},
				]
				resolve(nodes)
			}, 500)
		},
		// 提交
		submitForm(formName) {
			this.$refs[formName].validate(valid => {
				if (valid) {
					const areaArrId = []
					const areaCode = []
					const arr = this.$refs['areaSelect'].getCheckedNodes()
					for (let i = 0, len = arr.length; i < len; i++) {
						const areaNode = arr[i]
						if (areaNode) {
							areaArrId.push(areaNode.data.value2)
							areaCode.push(areaNode.data.value)
						}
					}
					this.ruleForm.areaId = areaArrId.join('/')
					this.ruleForm.areaCode = areaCode.join()
					console.warn(this.ruleForm.areaId)
					console.warn(this.ruleForm.areaCode)
				} else {
					return false
				}
			})
		},
	},
}
</script>
