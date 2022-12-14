<template>
<div class="container-tasklist">
    <h3 v-bind:class="task.is_complete ? 'completed' : 'not-completed'">{{ task.title }}</h3>
    <p v-bind:class="task.is_complete ? 'completed' : 'not-completed'">{{ task.description }}</p>

    <!---- Buttons ---->
    <div class="tasks-btn">
        <button @click="deleteTask" class="delete-btn">Delete</button>
        <button @click="editTaskFunction" class="edit-btn">Edit</button>
        <button @click="statusTask" class="status-btn">Done!</button>
    </div>

    <!---- Edit Input Tasks ---->
    <div v-show="editTask" class="task-edit">
        <input type="text" class="input-task" placeholder="New Title" v-model="name"/>
        <input type="text" class="input-task" placeholder="New Description" v-model="description" />

        <button @click="changeTask" class="save-btn">Save</button>
    </div>
</div>
</template>

<script setup>
import { ref } from 'vue';
import { useTaskStore } from '../stores/task';
import { supabase } from '../supabase';

const taskStore = useTaskStore();
const emit = defineEmits(["getTasksHijo"]);
const name = ref('');
const description = ref('');
const props = defineProps({
    task: Object,
});
// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.

const deleteTask = async() => {
    await taskStore.deleteTask(props.task.id);
    emit("getTasksHijo");
};

// ------- Function para cambiar Task --------- //

const changeTask = async () => {
    await taskStore.changeTask(name.value, description.value, props.task.id);
    editTask.value = false;
    emit("getTasksHijo");
};
const editTask = ref(false);
const editTaskFunction = () => {
    editTask.value = !editTask.value;
};

//------------ Function para marcar Complete - InComplete ------- //

const statusTask = async () => {
    await taskStore.statusTask(!props.task.is_complete, props.task.id);
    emit("getTasksHijo");
    console.log("el boleano es:" + props.task.is_complete)
    
};

</script>

<style></style>

<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.
-->
