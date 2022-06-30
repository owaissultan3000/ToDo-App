<template>
    <button @click="onClick()" :style="{background: color}" 
    class="btn">{{text}}</button>
</template>

<script>
export default {
    
    name:"Task-Button",
    props:{
        text:{
    default: "",
    type: String
            },
        color:{
    default: "",
    type: String
            }       
    },
    methods:{
        onClick()
        {
            this.$emit('toggle-task')
        }
    }
}
</script>
