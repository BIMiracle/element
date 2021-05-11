<template>
	<div style="margin: 20px;">
		<el-input v-model="input" placeholder="请输入内容"></el-input>

		<el-cascader ref="institution" v-model="listQuery.institutionId" :props="institutionProps" :show-all-levels="false" clearable style="width: 280px" @change="handleChange" />
	</div>
</template>

<script>
export default {
	data() {
		return {
			input: 'Hello Element UI!',
			listQuery: {
				institutionId: null,
			},
			institutionProps: {
				checkStrictly: true,
				lazy: true,
				lazyLoad: this.lazyLoad,
			},
			id: 0,
			id2: 1000,
		}
	},
	methods: {
		lazyLoad(node, resolve) {
      if (node.isLeaf) {
        resolve()
        return
      }
      console.warn('加载网络');
			setTimeout(() => {
				const nodes = [
					{
						value: this.id++,
						label: '01',
						leaf: true,
					},
					{
						value: this.id2++,
						label: '02',
						leaf: false,
					},
				]
				resolve(nodes)
			}, 1000)
		},
		handleChange(val) {
			console.warn(val)
		},
	},
}
</script>
