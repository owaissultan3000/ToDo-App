<template>
<div class="container">
<Header @toggle-task="toogleTask" :showaddTask="showaddTask" title="Todo App"/>
<div v-if="showaddTask">
<AddTask @add-task="addTask"/>
</div>
<Tasks @toggle-remainder="toggleRemainder" @delete-task="deleteTask" :tasks="tasks"/>
</div>
  
</template>

<script>
import Header from './components/Task-Header'
import Tasks from './components/Tasks-Preview'
import AddTask from './components/Add-Task'



export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data()
  {
    return{
      tasks:[],
      showaddTask:false
    }
  },
  methods: {
    deleteTask(id)
    {
      if(confirm("Are you sure you want to delete this task?"))
      {
          this.tasks = this.tasks.filter((task) =>task.id !== id )
      }
    },
    toggleRemainder(id)
    {
      this.tasks = this.tasks.map((task) => task.id === id ? {...task,reminder:!task.reminder} : task)
    },
    addTask(task)
    {
      this.tasks = [...this.tasks,task]
    },
    toogleTask()
    {
      this.showaddTask = !this.showaddTask
    }
  },
  created()
  {
    this.tasks = [
     {
      id: 1,
      text: 'Doctors Appointment',
      day: "March 1st at 2:30pm",
      reminder: true,
    },
     {
      id: 2,
      text: 'Assignment',
      day: "March 1st at 3:30pm",
      reminder: false,
    },
     {
      id: 3,
      text: 'Maintanence',
      day: "March 1st at 4:30pm",
      reminder: true,
    },
  ]
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
