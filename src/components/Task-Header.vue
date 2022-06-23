<template>
    <header>
       <h1>{{title}}</h1> 
       <Button @toggle-task="$emit('toggle-task')" text="Add Task" color="green"/>
    </header>
</template>

<script>
import Button from './Task-Button'
export default {
    
    name:'Task-Header',
    props:{
        title: String,
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