<template>
    <div class="container">
        <h1>Todo Page</h1>
        <div v-if="loading">
            Loading...
        </div>
        <form v-else @submit.prevent="onSave">
            <div class="row1">
                <div>
                    <div class="form-group">
                        <label>Todo Subject</label>
                        <input type="text" class="from-control" v-model="todo.subject">
                    </div>
                </div>
                <div>
                    <div class="form-group">
                        <label>Status</label>
                        
                        <div>
                            <button @click="toggleTodoStatus" type="button" class="btn" :class="todo.completed ? 'btnGG':'btnRR'">{{todo.completed ? '완료':'미완료'}}</button>
                        </div>
                    </div>
                </div>
            </div>
            <button class=" btnmt" type="submit"  :disabled="!todoUpdated">저장</button>
            <button class="btn btnW btnl" @click="moveToTodoListPage">취소</button>
        </form>
    </div>
</template>

<script>
    import {useRoute, useRouter} from 'vue-router';
    import axios from 'axios';
    import { ref, computed} from 'vue';
    import _ from 'lodash';
    export default {
        setup() {
            const route=useRoute();
            const router=useRouter();
            const todo=ref('');
            const loading= ref(true);
            const originalTodo=ref(null);
            const todoId=route.params.id

            //console.log(route.params.id)
            const getTodo = async () =>{
                const res = await axios.get(`http://localhost:3000/todos/${todoId} ` );
                todo.value=res.data;
                loading.value=false;
                originalTodo.value = {...res.data} 
            }
           

            const toggleTodoStatus = () =>{
                todo.value.completed = !todo.value.completed;
            }
            const moveToTodoListPage =() =>{
                router.push({
                    name:'Todos'
                })
            }
            getTodo();

            const onSave = async () => {
                const res = await axios.put(`http://localhost:3000/todos/${todoId}`, {
                    subject:todo.value.subject,
                    completed:todo.value.completed
                    
                });
                originalTodo.value = {...res.data} 
                console.log(res)
            }
            const todoUpdated = computed(() => {
                return !_.isEqual(todo.value, originalTodo.value)
            }) 
            return {
                todo,
                toggleTodoStatus,
                moveToTodoListPage,
                onSave,
                todoUpdated,
            }
        }
    }
</script>
<style>
    h1{margin-top: 30px; margin-bottom: 20px;}
    .form-group{
        display: flex;
        flex-direction: column;
    }
    .form-group label{ font-weight: bold; margin-bottom: 5px;}
    .form-group .from-control{padding: 10px 20px;}
    .btnmt{margin-top: 10px;}
    .row1{display: flex; justify-content: space-between;}
    .row1>div{flex-basis: 49%; }
    .btnRR{padding: 10px 20px; background: red;}
    .btnGG{padding: 10px 20px; background: rgb(7, 104, 112);}
    .btnW{background: rgb(201, 201, 201); color: #222}
    .btnl{margin-left: 10px;}
</style>
<style scoped>
    .row1{margin-bottom: 20px;}
</style>