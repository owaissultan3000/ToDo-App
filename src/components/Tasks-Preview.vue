<template>
    <div :key="task.id" v-for="task in tasks">
        <Tasks @toggle-remainder="$emit('toggle-remainder', task.id)" @delete-task="$emit('delete-task',task.id)" :task="task" />
        
    </div>
    
</template>

<script>
import Tasks from './Single-Task.vue'
export default {
    name:'Tasks-Preview',
    props:{
        tasks:Array
    },
    components:{
    Tasks    
    },
    emits:['delete-task','toggle-remainder']
}

</script>