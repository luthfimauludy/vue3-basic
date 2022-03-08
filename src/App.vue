<template>
    <div class="container" style="margin-top: 20px">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">SIMPLE TODOLIST</h5>
                <form @submit.prevent="add()">
                    <div class="row">
                        <div class="col-10">
                            <input type="text" class="form-control" v-model="todo" placeholder="Please type here" />
                            <div v-if="errors.err" class="error-messages">
                                <div v-for="(error, index) in errors.err" :key="index">
                                    <small>{{ error }}</small>
                                </div>
                            </div>
                        </div>
                        <div class="col-2">
                            <button type="submit" class="btn btn-success">ADD</button>
                        </div>
                    </div>
                </form>
                <List 
                    :todos="todos.list" 
                    @deleteTodo="deleteTodo" 
                    @doneTodo="doneTodo" 
                />
                <br />
                <small>Total TODO : {{ todos.list.length }}</small>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, reactive } from "vue";
import List from "./components/List.vue";

export default {
    components: { List },
    setup() {
        const todo = ref("");
        const todos = reactive({
            list: []
        });
        const errors = reactive({
            err: []
        });

        // beforeMount(() => {
        //     if (localStorage.getItem("TODOS")) {
        //         todos.list = JSON.parse(localStorage.getItem("TODOS"));
        //     } else {
        //         localStorage.setItem("TODOS", "[]");
        //     }
        // });
            
        // updated(() => {
        //     localStorage.setItem("TODOS", JSON.stringify(todos.list));
        // });

        const add = () => {
            errors.err = [];

            if (!todo.value) {
                errors.err.push("Field is required!");
                return false;
            };

            todos.list.unshift({
                activity: todo.value,
                isDone: false
            });
            todo.value = "";
        };

        const deleteTodo = (todoIndex) => {
            todos.list.splice(todoIndex, 1);
        };

        const doneTodo = ([todoIndex, v]) => {
            todos.list[todoIndex].isDone = v;
            localStorage.setItem("TODOS", JSON.stringify(todos.list));
        };

        return { todo, todos, errors, add, deleteTodo, doneTodo };
    },
};
</script>
