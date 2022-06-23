<template>
    <header>
       <h1>{{title}}</h1> 
       <Button @toggle-task="$emit('toggle-task')" :text="showaddTask ? 'Close' : 'Add Task'" :color="showaddTask ? 'red' : 'green'"/>
    </header>
</template>

<script>
import Button from './Task-Button'
export default {
    
    name:'Task-Header',
    props:{
        title: String,
        showaddTask: Boolean,
    },
    components:{
        Button
    },
}
</script>

<style scoped>
header{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:20px;

}
</style>