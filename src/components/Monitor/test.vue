<template>
  <el-form :label-position="'right'" label-width="120px">

   <el-form-item label="运维配置新增:"></el-form-item>
   <el-form-item label="检查项类型:">
    <el-input ></el-input>
   </el-form-item>
   <el-form-item label="检查项特征:">
    <el-input ></el-input>
   </el-form-item>
   <el-form-item label="检查项描述:">
    <el-input ></el-input>
   </el-form-item>

   <el-form-item :inline="true" >
    <el-form-item label="输出标准参数">
     <el-input style="width:30%"></el-input>
    </el-form-item>
    <el-form-item label="输出标准值">
     <el-input style="width:30%"></el-input>
    </el-form-item>
   </el-form-item>




   <el-form-item
           prop="email"
           label="邮箱">
    <el-input ></el-input>
   </el-form-item>
   <el-form-item
           v-for="(domain, index) in dynamicValidateForm.domains"
           :label="'域名' + index"
           :key="domain.key"
           :prop="'domains.' + index + '.value'"
           :rules="{
      required: true, message: '域名不能为空', trigger: 'blur'
    }"
   >
    <el-input ></el-input><el-button @click.prevent="removeDomain(domain)">删除</el-button>
   </el-form-item>
   <el-form-item>
    <el-button type="primary" @click="submitForm('dynamicValidateForm')">提交</el-button>
    <el-button @click="addDomain">新增</el-button>
    <el-button @click="resetForm('dynamicValidateForm')">重置</el-button>
   </el-form-item>
  </el-form>
</template>

<style>
.el-input--suffix{
 display: inline;
 width:50%;
}

</style>

<script>
    export default {
        data() {
            return {
                dynamicValidateForm: {
                    domains: [{
                        value: ''
                    }],
                    email: ''
                }
            }
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            removeDomain(item) {
                var index = this.dynamicValidateForm.domains.indexOf(item)
                if (index !== -1) {
                    this.dynamicValidateForm.domains.splice(index, 1)
                }
            },
            addDomain() {
                this.dynamicValidateForm.domains.push({
                    value: '',
                    key: Date.now()
                });
            }
        }
    }
</script>