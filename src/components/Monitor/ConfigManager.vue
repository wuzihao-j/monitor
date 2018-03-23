<template>
  <el-form label-width="160px" id="myForm">

   <el-form-item label="运维配置新增:"></el-form-item>
   <el-form-item  style="margin-right: 40px">
    <el-form-item label="作业名称:">
     <el-input v-model="formObj.job_name" style="width:70%; margin-bottom: 10px"></el-input>
    </el-form-item>
   <el-form-item label="检查项类型:">
    <el-select v-model="value" placeholder="请选择" style="margin-bottom: 10px" @click="">
     <el-option
             v-for="item in options"
             :key="item.value"
             :label="item.label"
             :value="item.value">
     </el-option>
    </el-select>
     <!--<el-input v-model="formObj.check_type" style="width:70%; margin-bottom: 10px"></el-input>-->
    <el-button type="primary" style="margin-left: 20px" @click="router('checkFeatureAdd')">新增</el-button>
   </el-form-item>
    <el-form-item label="检查项特征:">
     <el-input v-model="formObj.check_feature_key"  style="width:70%; margin-bottom: 10px"></el-input>
    </el-form-item>
    <el-form-item label="检查项描述:">
     <el-input v-model="formObj.check_feature_desc"  style="width:70%; margin-bottom: 10px"></el-input>
    </el-form-item>
    <el-form-item label="上传Kettle文件:">
     <el-form-item label="文件上传" prop="pic">
      <input type="file" @change="getFile($event)"></input>
     </el-form-item>
    </el-form-item>
   </el-form-item>

   <el-form-item label="输出模板配置:"></el-form-item>
   <el-form-item
           v-for="item in formObj.dynamicList"
           :key="item.key"
   >
    <el-form-item :inline="true" >
     <span style="width:8%">输出标准参数{{item.index}}</span>
     <el-input style="width:30%" v-model="item.output_stardard_key"></el-input>
     <span style="width:8%">输出标准值{{item.index}}</span>
     <el-input style="width:30%" v-model="item.output_standard_value"></el-input>
     <el-button @click.prevent="removeDomain(item)" style="width:8%">删除</el-button>
    </el-form-item>
   </el-form-item>
   <el-form-item>
    <el-button type="primary" @click="submitForm($event)">提交</el-button>
    <el-button @click="addDomain">新增</el-button>
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
    export global from 'Global.vue'

    export default {
        data() {
            return {
                options: [],
                fileList: [],
                formObj: {
                    job_name: '',
                    check_type: '',
                    check_feature_key: '',
                    check_feature_desc: '',
                    kettle_file: '',
                    dynamicList: [{
                        output_stardard_key: '',
                        output_standard_value: ''
                    }],
                },
                currentIndex: 1
            }
        },
        methods: {
            getFile(event) {
                this.formObj.kettle_file = event.target.files[0];
            },
            submitForm(event) {
                event.preventDefault();
                let formData = new FormData();
                formData.append('job_name', this.formObj.job_name);
                formData.append('check_type', this.formObj.check_type);
                formData.append('check_feature_key', this.formObj.check_feature_key);
                formData.append('check_feature_desc', this.formObj.check_feature_desc);
//                formData.append('dynamicList', this.formObj.dynamicList);
                formData.append('file', this.formObj.kettle_file);

                let config = {
                    headers: {
                        'Content-Type': 'multipart/form-data;application/json'
                    }
                }
                this.$http.post(Global.PATH + '/kettle/saveKettleInfo', formData, config).then(function (res) {
                    if (res.status === 2000) {
                        /*这里做处理*/
                    }
                })
            },
            removeDomain(item) {
                var index = this.formObj.dynamicList.indexOf(item)
                if (index !== -1) {
                    this.currentIndex -= 1;
                    this.formObj.dynamicList.splice(index, 1)
                }
            },
            addDomain() {
                this.currentIndex += 1;
                this.formObj.dynamicList.push({
                    key: Date.now(),
                    output_stardard_key: '',
                    output_standard_value: '',
                    index: this.currentIndex
                });
            },
            handleRemove(file, fileList) {
                console.log(file, fileList);
            },
            handlePreview(file) {
                console.log(file);
            },
            handleExceed(files, fileList) {
                this.$message.warning(`当前限制选择 1 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
            },
            beforeRemove(file, fileList) {
                return this.$confirm(`确定移除 ${ file.name }？`);
            },
            submitUpload() {
                this.$refs.upload.submit();
            },
            router(url){
                this.$router.replace({path: url})
            },
            mounted(){
                this.$http.post(Global.PATH + '/kettle/checkType/list', formData, config).then(function (res) {
                    this.options = res.data.data;
                })
            }
        }
    }
</script>