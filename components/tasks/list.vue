<template>
    <div class="tasks-list flex --direction-column">
        <div
            v-for="(task, index) in tasksList"
            :key="task.id"
        >
            <TasksItem 
                v-model:task="tasksList[index]"
                v-model:tasksList="tasksList"
            />
        </div>
    </div>
</template>

<style>
    @import '~/components/tasks/__tasks.css';
</style>

<script setup lang="ts">
    import type { ITask } from './model';

    const tasksList = defineModel<ITask[]>('tasksList')

</script>