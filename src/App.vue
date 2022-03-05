<template>
    <div class="container" style="margin-top: 20px">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">SIMPLE TODOLIST</h5>
                <form @submit.prevent="add()">
                    <div class="row">
                        <div class="col-10">
                            <input type="text" class="form-control" v-model="todo" placeholder="Please type here" />
                            <div v-if="errors" class="error-messages">
                                <div v-for="(error, index) in errors" :key="index">
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
                    :todos="todos" 
                    @deleteTodo="deleteTodo" 
                    @doneTodo="doneTodo" 
                />
                <br />
                <small>Total TODO : {{ todos.length }}</small>
            </div>
        </div>
    </div>
</template>

<script>
import List from "./components/List.vue";

export default {
    components: { List },
    data() {
        return {
            todo: "",
            todos: [],
            errors: []
        };
    },
    beforeMount() {
        if (localStorage.getItem("TODOS")) {
            this.todos = JSON.parse(localStorage.getItem("TODOS"));
        } else {
            localStorage.setItem("TODOS", "[]");
        }
    },
    updated() {
        localStorage.setItem("TODOS", JSON.stringify(this.todos));
    },
    methods: {
        add() {
            this.errors = [];

            if (!this.todo) {
                this.errors.push("Field is required!");
                return false;
            }

            this.todos.unshift({
                activity: this.todo,
                isDone: false
            });
            this.todo = "";
        },
        deleteTodo(todoIndex) {
            this.todos.splice(todoIndex, 1);
        },
        doneTodo([todoIndex, v]) {
            this.todos[todoIndex].isDone = v;
            localStorage.setItem("TODOS", JSON.stringify(this.todos));
        }
    }
};
</script>
