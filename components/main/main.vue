<template>
    <div class="center-wrap col --12">
        <div class="add-task flex --gap-4">
            <input 
                class="input --border" 
                type="text" 
                placeholder="Введите название задачи"
                v-model="nameTask"
                @keyup.enter="addTask"
            >
            <button 
                class="add-task__button"
                @click="addTask"
                :disabled="nameTask.length < 1"
            >
                Добавить
            </button>
        </div>
        <div class="tasks__filter flex">
            <div v-for="option in filterOptions"
                class="tasks__filter-option"
                @click="activeOption = option.value"
                :class="{'--active': activeOption === option.value}"
            >
                {{ option.text }}
            </div>
        </div>
        <TasksList
            v-model:tasksList="filterTasksList"
        />
    </div>
</template>

<style>
    @import '~/components/main/__main.css';
</style>

<script setup lang="ts">
    import type { ITask } from '../tasks/model';
    const tasksList = ref<ITask[]>([])
    const nameTask = ref<string>('');
    const activeOption = ref('all')
    const filterOptions = [{value: 'all', text: 'Все задачи'},{value: 'true', text: 'Выполненные задачи'},{value: 'false', text: 'Невыполненные задачи'},]

    function addTask(){
        tasksList.value.unshift({id:tasksList.value.length + 1,check: false,title:nameTask.value} as ITask)
        localStorage.setItem('task', JSON.stringify(tasksList.value))
        nameTask.value = ''
    }

    const showLocalStorageContent = () => {
        if(localStorage.getItem('task')){
            tasksList.value = JSON.parse(localStorage.getItem('task'))
        }
    }

    const filterTasksList = computed(() => {
        const filterTasks = tasksList.value.filter((task) => {
			return task.check.toString() == activeOption.value || activeOption.value === 'all'
        })

        return filterTasks
    })
  
    onMounted(() => {
        showLocalStorageContent()
    })
</script>