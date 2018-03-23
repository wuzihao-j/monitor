<template>
    <div>
            <span>检查项类型：</span>
            <el-input v-model="check_feature_key" style="width:70%; margin-bottom: 10px"></el-input>
            <el-button type="success">确定</el-button>
            <el-button type="danger" @click="router('configAdd')">取消</el-button>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                check_feature_key: ''
            }
        },
        methods: {
            confirm(){

            },
            router(url){
                this.$router.replace({path: url})
            }
        }
    }
</script>