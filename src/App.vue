<template>
    <div class="container" style="margin-top: 20px">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">SIMPLE TODOLIST</h5>
                <div class="row">
                    <div class="col-10">
                        <input type="text" class="form-control" v-model="todo" placeholder="Ketik disini" @keyup.enter="add"/>
                        <div v-if="errors" class="error-messages">
                            <div v-for="(error, index) in errors" :key="index">
                                <small>{{ error }}</small>
                            </div>
                        </div>
                    </div>
                    <div class="col-2">
                        <button class="btn btn-success" @click="add">ADD</button>
                    </div>
                </div>
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
                this.errors.push("Data tidak boleh kosong!");
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
