<template>
    <div class="container" style="margin-top: 20px">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">SIMPLE TODO APP</h5>
                <div class="row">
                    <div class="col-10">
                        <input v-model="todo" type="text" class="form-control" placeholder="Ketik disini" @keyup.enter="add"/>
                        <div class="error" v-if="error"><small>{{ error }}</small></div>
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
                <small>Total TODO : {{ totalTODO }}</small>
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
            error: ''
        };
    },
    mounted() {
        this.todos = JSON.parse(localStorage.getItem('todos'));
    },
    computed: {
        totalTODO() {
            return this.todos.length;
        }
    },
    methods: {
        add() {
            if (this.todo === '') {
                this.error = 'Data tidak boleh kosong!';
                return false;
            }
            this.todos.unshift({
                activity: this.todo,
                isDone: false
            });
            this.todo = "";
            this.saveToLocalStorage();
        },
        deleteTodo(todoIndex) {
            this.todos = this.todos.filter((item, index) => {
                if (index != todoIndex) {
                    return item;
                }
            });
            this.saveToLocalStorage();
        },
        doneTodo(todoIndex) {
            this.todos = this.todos.filter((item, index) => {
                if (index == todoIndex) {
                    item.isDone = true;
                }
                return item;
            });
            this.saveToLocalStorage();
        },
        saveToLocalStorage() {
            localStorage.setItem('todos', JSON.stringify(this.todos));
        }
    }
};
</script>
