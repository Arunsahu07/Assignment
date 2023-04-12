<script setup>
import { ref } from 'vue'
const todo = ref({ name: '', folder: 'select Folder', isCompleted: false })
const folders = ref(new Map())
const newFolderName = ref('')
const handleSubmit = () => {
    console.log(todo.value.folder);
    if (!todo.value.name || todo.value.folder === 'select Folder') {
        alert("Please Type Todo and select folder.")

    }
    else {

        folders.value.set(todo.value.folder, [...folders.value.get(todo.value.folder), { name: todo.value.name, isCompleted: false }])
        todo.value = { name: '', folder: 'select Folder', isCompleted: false }
    }

}
const handleFolderCreate = () => {
    if (!newFolderName.value)
        alert('Please Enter folder name.')
    else {

        folders.value.set(newFolderName.value, [])
        newFolderName.value = ''
    }


}
</script>
<template>
    <form @submit.prevent="handleFolderCreate">
        <input placeholder="Create New Folder" v-model="newFolderName" />
        <button type="submit">Create folder</button>
    </form>
    <form @submit.prevent='handleSubmit'>
        <input v-model="todo.name" placeholder="add todo" />
        <select v-model="todo.folder">
            <option selected value="select Folder" disabled="true">Select folder</option>
            <option v-for="(key, idx) in  Array.from(folders.keys())" key="idx" :value="key">{{ key }}</option>
        </select>
        <button type="submit">Add ToDo</button>
    </form>
    <div class="folder-container">
        <div v-for="folder in Array.from(folders.keys())" key="folder">
            <h2>{{ folder }}</h2>
            <div v-for="(todo, idx) in Array.from(folders.get(folder))" key="idx" class="todo-container">
                <input type="checkbox" :disabled="todo.isCompleted" v-model="todo.isCompleted" />
                <div :class="todo.isCompleted ? 'completed' : ''">{{ todo.name }}</div>
            </div>
        </div>
    </div>
</template>
<style scoped>
.todo-container,
.folder-container {
    display: flex;

}

.folder-container {

    gap: 2rem;
}

.completed {
    text-decoration: line-through;
}
input{
    margin:0.5rem;
    outline: none;
    border-radius: 5px;
    padding: 5px;
}
button, select{
    border-radius: 5px;
    padding: 5px 10px;
    background-color: #288FFF;
    color:white;
    border: none;
}
select{
    margin-right: .5rem;
}
</style>