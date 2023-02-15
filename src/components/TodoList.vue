<template>
    <div>
        <div v-for="(todo, index) in todos" :key="todo.id" class="card" >
            <div class="card-body">
                <div class="form-check">
                    <input type="checkbox" class="form-check-input" 
                    :value="todo.completed" @change="toggleTodo(index)">
                    <!-- <label class="form-check-label" :style="todo.completed ? todoStyle : {}"> -->
                    <label class="form-check-label" :class="{todoStyle:todo.completed}">
                        {{ todo.subject }}
                    </label>
                </div>
                <div>
                    <button class="btnR" @click="deleteTodo(index)">삭제</button>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            todos:{
                type:Array,
                required: true
            }
        },
        emits:["toggleTodo","delete-todo"],
        setup(props, context){
            const toggleTodo = (index) =>{
                context.emit('toggle-todo', index)
            }
            const deleteTodo = (index) =>{
                context.emit('delete-todo', index)
            }
            return {
                deleteTodo,
                toggleTodo,
            }
        }
    }
</script>
