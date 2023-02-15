<template>
    <div class="container">
        <h2>TODO-List</h2>
        <input type="text" placeholder="검색" class="form-control" v-model="serchText">
        <TodoSimpleForm  @add-todo="addTodo"/>
        <div v-if="!filteredTodos.length">
            찾는 문장이 없습니다..
        </div>
        <TodoList :todos="filteredTodos" @delete-todo="deleteTodo" @toggle-todo="toggleTodo" />
        <br>
        <nav class="nav">
          <ul class="pagination">
            <li class="page-item"><a href="#" class="page-link">🧲</a></li>
          </ul>
        </nav>
    </div>
</template>

<script>
import { computed, ref } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';
import '@fortawesome/fontawesome-free/js/all.js'


export default {
    components: {
        TodoSimpleForm,
        TodoList,
    },
    setup(){
        
        const todos = ref([ ]);
        const serchText= ref('');
        const error=ref('');
        

       

        const getTodos = async () =>{
          try{
             const res=await axios.get('http://localhost:3000/todos')
             console.log(res.data)
             todos.value=res.data
          }catch(err){
            console.error(err);
            err.value = "찾는문장이없습니다."
          }
        };
        getTodos()
        
        const addTodo = async (todo) => {
            error.value='';
            try{
                const res=await axios.post('http://localhost:3000/todos', {
                    subject:todo.subject,
                    completed:todo.completed, 
                })
                todos.value.push(res.data);
            }catch (err){
                console.log(err);
                error.value="잘못된 입력입니다."
            }
            
        }

        /* const addTodo = (todo) => {
           // todos.value.push(todo);
            error.value='';
           axios.post('http://localhost:3000/todos',{
                subject:todo.subject,
                completed:todo.completed,
           }).then((res) =>{
                console.log(res);
                todos.value.push(res.data)
           }).catch((err) =>{
                console.log(err);
                error.value="잘못된 입력입니다."
           });
        } */
        const deleteTodo = async (index) => {
          error.value = "찾는문장이 없음"
          const id=todos.value[index].id;
          try{
             await axios.delete(`http://localhost:3000/todos/${id}`)
             todos.value.splice(index, 1)
          }catch(err){
            console.log(err);
            error.value="잘못된 입력입니다."
          }
            
        }
        const toggleTodo= async (index) =>{
           // console.log(index)
           error.value="";
           const id= todos.value[index].id;

           try{
                await axios.patch('http://localhost:3000/todos/'+ id, {
                    completed: !todos.value[index].completed
                });
                todos.value[index].completed= !todos.value[index].completed;
           }catch(err){
                console.log(err);
                error.value="찾는 문장이 없습니다"
           }
        }
        const filteredTodos = computed(() =>{
            if(serchText.value){
                return todos.value.filter(todo =>{
                    return todo.subject.includes(serchText.value);
                })
            }
            return todos.value;
        })

        return {
            todos,
           /*  todoStyle, */
           deleteTodo,
           addTodo,
           toggleTodo,
           serchText,
           filteredTodos,
           getTodos
        }
    }
}
</script>

<style>
    *{margin: 0; padding: 0; box-sizing: border-box;}
    .container{width: 100%; max-width: 1024px; margin: 0 auto; padding: 0 20px;}
    h2{text-align: center; color: #147f8d; margin-bottom: 50px; margin-top: 50px;}
    .d-flex{display: flex;}
    .flex-grow-1{flex-grow: 1;}
    .flex-grow-1 input{width: 98%; padding: 10px 20px;}
    .btn{padding: 12px 30px; border: none; background: #147f8d; color: #fff}
    form{margin-bottom: 50px;}
    .card{margin: 10px 0;}
    .card-body{border: 1px solid #ccc; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center;}
    .form-check-input{margin-right: 10px;}
    .todoStyle{text-decoration: line-through;color: gray}
    .btnR{padding: 5px 20px; background: #e93838; color: #fff; border: none}
    .form-control{width: 100%; border: 1px solid #ddd; margin-bottom: 10px; padding: 10px 20px;}
</style>