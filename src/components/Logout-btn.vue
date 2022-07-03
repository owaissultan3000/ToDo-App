<template>
    <button @click="onClick()" 
    class="btn">Logout</button>
</template>

<script>
export default {
    
    name:"Task-Button",
    props:{  
    },
    methods:{
        onClick()
        {
            this.$emit("remove-token")
        }
    }
}
</script>
<style scoped>
.btn{
background:green;
margin-left: 75%;
margin-top: 15px;
}

</style>
