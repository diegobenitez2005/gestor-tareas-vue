<template>
    <div>
        <el-col :span="12">
            <el-card class="box-card" shadow="hover" style="margin: 5px 0">
                <el-row :gutter="12">
                    <el-col :span="21">{{ todo }}</el-col>
                    <el-col :span="3">
                        <el-button @click="$emit('delTodo')" type="success" icon="el-icon-check" circle></el-button>
                    </el-col>
                </el-row>
            </el-card>
        </el-col>
    </div>
</template>

<script>
export default {
    name: 'TodoItem',
    data() {
        return {
            todo: ""
        }
    },
    props: {
        title: String,
    },
    created() {
        this.todo = this.title
    }

}
</script>