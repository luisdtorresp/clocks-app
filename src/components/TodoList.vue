

<template>
    <div>
        <h2>Todo List</h2>
        <q-list>
            <q-item v-for="(todo, index) in todos" :key="index">
                <q-item-section>{{ todo.text }}</q-item-section>
                <q-item-section side>
                    <q-btn @click="removeTodo(index)" color="negative" icon="delete" flat round dense />
                </q-item-section>
            </q-item>
        </q-list>
        <q-input v-model="newTodo" @keyup.enter="addTodo" outlined>
            <template v-slot:append>
                <q-icon name="add" @click="addTodo" class="cursor-pointer" />
            </template>
        </q-input>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTodo: '',
            todos: []
        }
    },
    methods: {
        addTodo() {
            this.todos.push({ text: this.newTodo });
            this.newTodo = '';
        },
        removeTodo(index) {
            this.todos.splice(index, 1);
        }
    }
}
</script>

<style scoped>
/* Add your CSS here */
</style>
