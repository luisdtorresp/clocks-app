

<template>
    <div>
        <h2>Todo List</h2>
        <q-list>
            <q-item v-for="(todo, index) in todos" :key="index">
                <q-item-section dense>
                    
                    <q-checkbox :false-value="null" :indeterminate-value="''"  v-model="todo.checked" />
                </q-item-section>
                <q-item-section width="100%">
                    
                    <span v-if="!todo.edit" :class="{ 'completed': todo.checked }">{{ todo.text }}</span>
                    <q-input v-else v-model="todo.text" @keyup.enter="todo.edit = false" width="400px" />

                    
                </q-item-section>
                <q-item-section side>
                    <q-btn v-if="!todo.edit" @click="todo.edit = true" color="primary" icon="edit" flat round dense />
                    <q-btn v-if="todo.edit" @click="removeTodo(index)" color="negative" icon="delete" flat round dense />
                    <q-btn v-if="todo.checked && !todo.edit" @click="removeTodo(index)" color="green" icon="check" flat round dense />
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
            // if newTodo is empty or only whitespace, return
            if (this.newTodo.trim() === '') return;
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
.completed {
    text-decoration: line-through;
    opacity: 0.7;
}

[contenteditable]:focus  {
    outline: none;
    border: 2px solid orange;
    padding: 1rem;
    font-size: 1rem;
    box-sizing: border-box;
}
</style>
