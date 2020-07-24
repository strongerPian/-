<template>
	<div class="home">
		<el-row>
			<el-button type="primary" @click="Add">新增</el-button>
		</el-row>
		<!-- <el-input v-model="input" ref="search" placeholder="请输入内容"></el-input> -->
		<el-input
			v-model="search"
			style="display: inline-block;width: 1300px"
			placeholder="请输入搜索内容"
		>
		</el-input>
		<el-table ref="tableData" :data="tables" style="width: 100%" stripe hover>
			<el-table-column type="index" label="序号" width="50"></el-table-column>
			<el-table-column prop="name" label="名称" width="180"></el-table-column>
			<el-table-column prop="img" label="主图" width="180">
				<template slot-scope="scope">
					<img
						style="width:80px; max-heigth: 80px; display: block;margin: 0 auto;"
						:src="scope.row.img"
					/>
				</template>
			</el-table-column>

			<el-table-column prop="quantity" label="库存" width="180"></el-table-column>
			<el-table-column prop="price" label="价格" width="180"></el-table-column>
			<el-table-column label="操作" width="150">
				<template slot-scope="scope">
					<el-button size="mini" @click="handleEdit(scope.$index)">编辑</el-button>
					<el-button size="mini" type="danger" @click="handleDelete(scope.$index)"
						>删除</el-button
					>
				</template>
			</el-table-column>
		</el-table>
	</div>
</template>

<script>
export default {
	name: "Home",
	components: {
		// HelloWorld
	},
	data() {
		return {
			search: "",
			tableData: [
				{
					name: "",
					img: "",
					quantity: "",
					price: "",
				},
			],
		}
	},
	methods: {
		Add() {
			this.$router.push({
				name: "New",
			})
		},
		handleEdit(index) {
			this.$router.push({
				name: "Edit",
				query: { index },
			})
		},
		handleDelete(index) {
			this.$confirm("此操作将永久删除该数据, 是否继续?", "提示", {
				confirmButtonText: "确定",
				cancelButtonText: "取消",
				type: "warning",
			})
				.then(() => {
					let data = JSON.parse(localStorage.getItem("data"))
					data.splice(index, 1)
					localStorage.setItem("data", JSON.stringify(data))
					this.loadData()
					this.$message({
						type: "success",
						message: "删除成功!",
					})
				})
				.catch(() => {
					this.$message({
						type: "info",
						message: "已取消删除",
					})
				})
		},
		loadData() {
			let data = JSON.parse(localStorage.getItem("data"))
			// this.list = data
			this.tableData = data
		},
	},
	created() {
		this.loadData()
	},
	computed: {
		tables() {
			const search = this.search
			if (search) {
				return this.tableData.filter((data) => {
					return Object.keys(data).some((key) => {
						return (
							String(data[key])
								.toLowerCase()
								.indexOf(search) > -1
						)
					})
				})
			}
			return this.tableData
		},
	},
}
</script>
