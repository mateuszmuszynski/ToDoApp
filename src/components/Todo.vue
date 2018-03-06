<template>
    <div>
        <el-header>
            <div>Todo list</div>
        </el-header>
        <el-main>
            <div style="width: 50%; margin: 0 auto">
                <el-row>
                    <el-col :span="21">
                        <el-input placeholder="What needs to be done?" v-model="inputData"></el-input>
                    </el-col>
                    <el-col :span="3">
                        <el-button style="margin-left: 10px" @click="addTodoItem">Add</el-button>
                    </el-col>
                </el-row>
                <el-table :data="tableData">
                    <el-table-column align="left">
                        <template slot-scope="scope">
                            <el-checkbox v-model="scope.row.checked">{{scope.row.name}}</el-checkbox>
                        </template>
                    </el-table-column>
                    <el-table-column align="right">
                        <template slot-scope="scope">
                            <el-button size="mini" type="danger" @click="deleteTodoItem(scope.row)">Delete</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </div>
        </el-main>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data: function() {
        return {
            inputData: "",
            tableData: []
        }
    },
    methods: {
        addTodoItem: function() {
            this.tableData.push({ name: this.inputData, checked: false});
            this.inputData = "";
        },
        deleteTodoItem: function(item) {
            this.tableData.splice(this.tableData.indexOf(item), 1);
        }
    },
    mounted: function () {
        this.$nextTick(function () {
            axios.get('/api/todos').then(function(response) {
                this.tableData = response.data;
            });
        })
    }
}
</script>
