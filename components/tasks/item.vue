<template>
    <div class="tasks-item flex --align-center" :class="{'--check': task?.check === true}">
        <input 
            class="tasks-item__check"
            type="checkbox"
            @click="toogleCheck"
            :checked="task?.check === true"
        />
        <input 
            v-if="editActive"
            class="tasks-item__title --edit"
            autofocus
            type="text"
            v-model="task.title"
        />
        <h2 v-else class="tasks-item__title">{{ task?.title }}</h2>
        <button 
        v-if="editActive"
            class="tasks-item__save thumb"
            @click="saveEdit()"
        >
            <NuxtIcon name="check" filled/>
        </button>
        <button v-else
            class="tasks-item__edit thumb"
            @click="editActive = true"
        >
            <NuxtIcon name="edit" filled/>
        </button>
        <button 
            class="tasks-item__trash thumb"
            @click="trashTask()"
        >
            <NuxtIcon name="delete" filled/>
        </button>
    </div>
</template>

<script setup lang="ts">
    import type { ITask } from './model';

    const task = defineModel<ITask>('task')
    const tasksList = defineModel<ITask[]>('tasksList')
    const editActive = ref(false)

    function toogleCheck(){
        task.value.check = !task.value?.check
        tasksList.value.push(tasksList.value.splice(tasksList.value.indexOf(task.value), 1)[0])
        localStorage.setItem('task', JSON.stringify(tasksList.value))
    }

    function trashTask(){
        tasksList.value.splice( tasksList.value.indexOf(task.value) , 1)
        localStorage.setItem('task', JSON.stringify(tasksList.value))
    }

    function saveEdit(){
        localStorage.setItem('task', JSON.stringify(tasksList.value))
        editActive.value = false
    }

</script>