<template>
    <div class="container" style="margin-top: 20px">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">SIMPLE TODO APP</h5>
                <div class="row">
                    <div class="col-10">
                        <input v-model="todo" type="text" class="form-control" @keyup.enter="add"/>
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
        };
    },
    computed: {
        totalTODO() {
            return this.todos.length;
        }
    },
    methods: {
        add() {
            this.todos.unshift({
                activity: this.todo,
                isDone: false
            });
            this.todo = "";
        },
        deleteTodo(todoIndex) {
            this.todos = this.todos.filter((item, index) => {
                if (index != todoIndex) {
                    return item;
                }
            });
        },
        doneTodo(todoIndex) {
            this.todos = this.todos.filter((item, index) => {
                if (index == todoIndex) {
                    item.isDone = true;
                }
                return item;
            });
        }
    }
};
</script>
