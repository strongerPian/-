<template>
	<div class="edit">
		<el-form
			:model="numberValidateForm"
			ref="numberValidateForm"
			label-width="100px"
			class="demo-ruleForm"
		>
			<el-form-item
				prop="name"
				label="名称"
				:rules="[{ required: true, message: '名称不能为空' }]"
			>
				<el-input
					type="name"
					v-model.number="numberValidateForm.name"
					autocomplete="off"
				></el-input>
			</el-form-item>
			<el-form-item label="主图" prop="img" :rules="[{ required: false }]">
				<el-input
					type="img"
					v-model.number="numberValidateForm.img"
					autocomplete="off"
				></el-input>
			</el-form-item>
			<el-form-item label="库存" prop="quantity" :rules="[{ required: false }]">
				<el-input
					type="quantity"
					v-model.number="numberValidateForm.quantity"
					autocomplete="off"
				></el-input>
			</el-form-item>
			<el-form-item label="价格" prop="price" :rules="[{ required: false }]">
				<el-input
					type="price"
					v-model.number="numberValidateForm.price"
					autocomplete="off"
				></el-input>
			</el-form-item>
			<el-form-item>
				<el-button type="primary" @click="submitForm('numberValidateForm')"
					>提交</el-button
				>
				<el-button @click="resetForm('numberValidateForm')">重置</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
export default {
	data() {
		return {
			numberValidateForm: {
				name: "",
				img: "",
				quantity: "",
				price: "",
			},
		}
	},
	methods: {
		loadData() {
			let id = this.$route.query.index
			let data = JSON.parse(localStorage.getItem("data"))
			this.numberValidateForm = data[id]
		},
		submitForm() {
			if (this.numberValidateForm.name == "") {
				this.$alert("名称不能为空", {
					confirmButtonText: "确定",
				})
				return
			}
			let id = this.$route.query.index
			let data = JSON.parse(localStorage.getItem("data"))
			data.splice(id, 1, this.numberValidateForm)
			localStorage.setItem("data", JSON.stringify(data))
			this.$router.push({
				name: "Home",
			})
		},
		resetForm() {
			this.numberValidateForm = []
		},
	},
	created() {
		this.loadData()
	},
}
</script>

<style></style>
