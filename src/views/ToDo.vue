<template>
    <div class="todo">
        <h1>todolist</h1>
        <v-row>
            <v-col :cols="6">
                <v-text-field placeholder="please input" v-model="input"></v-text-field>
            </v-col>
            <v-col :cols="3">
                <v-btn color="primary" @click="submit">添加</v-btn>
            </v-col>
        </v-row>
        <h1>待完成</h1>
        <div>
            <v-card v-for="(thing,index) in thingList" :key="index" v-show="!thing.isDone">
                <v-card-title>
                    {{thing.title}}
                </v-card-title>
                <v-card-text>
                    {{thing.time}}{{thing.isDone}}
                </v-card-text>
                <v-card-actions>
                    <v-btn color="success" @click="doneChange(index)">完成</v-btn>
                </v-card-actions>
            </v-card>
        </div>
        <h1>已完成</h1>
        <div>
            <v-card v-for="(thing,index) in thingList" :key="index" v-show="thing.isDone">
                <v-card-title>
                    {{thing.title}}
                </v-card-title>
                <v-card-text>
                    {{thing.time}}{{thing.isDone}}
                </v-card-text>
                <v-card-actions>
                    <v-btn color="warning" @click="doneChange(index)">撤销</v-btn>
                    <v-btn color="error" @click="del(index)">删除</v-btn>
                </v-card-actions>
            </v-card>
        </div>
    </div>
</template>
<script>

export default {
     watch:{
        thingList: {
            handler: function(value){
                localStorage.setItem("thingList",JSON.stringify(value));
            },
            deep: true,
        }
    },
    created(){
        const listStr = localStorage.getItem("thingList");
        this.thingList = JSON.parse(listStr);
    }, 
    data() {
        return{
            input:"",
            thingList:[],
        }
    },
    methods: {
        submit(){
            let thing = {
                title: this.input,
                time: new Date().getTime(),
                isDone: false
            };
            this.thingList.push(thing);
            this.input="";
        },
        doneChange(index){
            this.thingList[index].isDone = !this.thingList[index].isDone;
        },
        del(index){
            this.thingList.splice(index,1);
        },
    }
}
</script>

