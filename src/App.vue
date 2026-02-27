<script setup>
import { ref } from 'vue';
import axios from 'axios';
const value=ref('')

getList()

const list=ref([])

async function getList() {
    const res=  await axios({
        url:'https://exhpc0821i.cloud.sealos.io/get_list',
        method: "GET"
    })
    list.value=res.data.list
}

async function add(){
    await axios({
        url: "https://exhpc0821i.cloud.sealos.io/add_todo",
        method: "POST",
        data:{
            value: value.value,
            isCompleted: false
        }
        
    })
    getList()
    value.value=''
  /*if (str.value==''){
    return ;
  }
  list.value.push({isComplete: false,
    text:str.value,})
  str.value=''
*/
}
async function del(/*index*/id){
    await axios ({
        url:"https://exhpc0821i.cloud.sealos.io/del_todo",
        method:"POST",
        data:{
            id:id
        }
    })
    getList()
  /*list.value.splice(index,1)*/
}

async function update (id) {
    await axios({
        url:"https://exhpc0821i.cloud.sealos.io/update_todo",
        method:"POST",
        data:{
            id,
        },
    })
    getList()
}
</script>


<template>
  <div class="todo-app">
    <div class="title">xxx的Todo App</div>
    <div class="todo-from">
        <input v-model="value" class="todo-input" type="text" placeholder="Add a todo">
        <div @click="add" class="todo-button">add todo</div>
    </div>
<!--
    <div class="item completed">
        <div>
        <input type="checkbox">
        <span class="name">吃饭</span>
        </div>

        <div class="del">del</div>
    </div>

    <div class="item">
        <div>
        <input type="checkbox">
        <span class="name">睡觉</span>
        </div>

        <div class="del">del</div>
    </div>
-->
    <div v-for="item in list" :class="[item.isCompleted?'completed':'item']">
        <div>
        <input @click="update(item._id)" v-model="item.isCompleted" type="checkbox">
        <span class="name">{{ item.value }}</span>
        </div>

        <div @click="del(/*index*/item._id)" class="del">del</div>
    </div>
</div>  <!--important!!!!!!!-->
</template>

<style scoped>
.completed{
        display: flex;

        text-decoration: line-through;
        opacity: 0.4;

        align-items: center; /*让框中所有元素再垂直方向上面居中*/
        justify-content: space-between ;


        box-sizing: border-box;
        width:80% ;
        height:50px ;
        border-radius:20px ;
        margin: 8px auto;
        padding: 16px;
        box-shadow: rgba(143, 151, 168, 0.2) 0px 8px 20px;
    }
    .del{
        color: red;
    }
    .item{
        display: flex;

        align-items: center; /*让框中所有元素再垂直方向上面居中*/
        justify-content: space-between ;


        box-sizing: border-box;
        width:80% ;
        height:50px ;
        border-radius:20px ;
        margin: 8px auto;
        padding: 16px;
        box-shadow: rgba(143, 151, 168, 0.2) 0px 8px 20px;
    }
    
    .todo-input{
        border: 1px solid #dfe1e5;
        outline:none;
        width: 60%;
        height: 50px;
        padding-left: 15px;
        border-radius: 20px 0 0 20px; /*每个角的位置按照顺时针来，左上 右上 左下 右下*/
    }
    .todo-from{
        display: flex; /*让盒子横着排列*/
        margin-top: 20px;
        margin-left: 30px;
        margin-bottom: 20px;
    }
    .todo-button{
        width:100px;
        height: 54px;
        border-radius: 0 20px 20px 0 ;
        text-align: center;
        background: linear-gradient(
            to right,
            rgb(98, 52, 152),
            rgba(44,114,251,1)
        ) ;
        padding-top:16px ;
        box-sizing:border-box;
        /*想要确保竖直方向居中我们还可以用 line-height: 52px;  只需要数值和height的数值一样就可以了*/
        color: #ffff;
        cursor: pointer;
        user-select: none;
    }
    body{
        background:linear-gradient(
            to right,
            rgb(98, 52, 152),
            rgba(44,114,251,1)
        );
    }
    .todo-app{
        width: 98%;  
        height:500px;
        background-color: #ffff;
        padding-top:30px;
        box-sizing:border-box;
        border-radius:5px;
        margin-top: 40px; /*这是box距离最上层的距离 */
        margin-left: 1%;  /*这是距离左边，因为width是百分之九十八，所以我们只需要弄成距离左边百分之一就可以确保框框在居中位置*/ 
    }
    .title {
        font-size: 30px;
        font-weight: 700;
        text-align: center;
        /*margin-top: ;   若果出现了margin塌陷的问题（也就是说margin用不了的时候），那就用padding-top（代码写在外层box里面），再组合用box-sizing来限制实际的外层框框大小*/

    }
</style>
