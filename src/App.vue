<template>
<div v-if="!authToken"  class="login">
<Login @user-cradentials="setToken"/>
</div>

<div v-else-if="authToken" class="container" >
<Header @toggle-task="toogleTask" :showaddTask="showaddTask" title="Todo App"/>
<div v-if="showaddTask">
<AddTask @add-task="addTask"/>
</div>
<Tasks @toggle-remainder="toggleRemainder" @delete-task="deleteTask" :tasks="tasks"/>
<Logout @remove-token="logout"/>
</div>
</template>

<script>
import Header from './components/Task-Header'
import Tasks from './components/Tasks-Preview'
import AddTask from './components/Add-Task'
import Login from './components/user-login'
import Logout from './components/Logout-btn'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
    Login,
    Logout
  },
  data()
  {
    return{
      tasks:[],
      showaddTask:false,
      authToken:"",
      userData:{}
    }
  },
  methods: {
    async getTasks(){
      const id = this.userData['id']
      const url = 'http://127.0.0.1:8000/api/todo-task/'+ id
      await fetch(url, {
                    method: "GET",
                })
                    .then((response) => response.json())
                    .then((data) => {
                        Object.values(data.data).forEach(item => this.tasks.push(item))
                    });

    },
   async setToken(data)
    {
      this.authToken = data.token;
      this.userData = data;
      this.tasks=[]
      this.getTasks()
    },
    
    async logout()
    {
      if(this.authToken != "") {
                await fetch("http://localhost:8000/api/logout/", {
                    method: "POST",
                    headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/x-www-form-urlencoded',  
                    'Authorization':'Token ' + this.authToken,
                    },
                })
                    .then(() => {
                      this.authToken=""
                      this.tasks=[]
                      console.clear()
                      
                    });

                } else {
                    console.log("Something went wrong. Please try agains");
                }
    },
   async deleteTask(id)
    {
      
      if(confirm("Are you sure you want to delete this task?"))
      {
          this.tasks = this.tasks.filter((task) =>task.id !== id )
          const url = 'http://127.0.0.1:8000/api/todo-task/'+ id
          await fetch(url, {
                    method: "DELETE",
                })
                    .then((response) => response.json())
                    .then((data) => {
                        console.log(data)
                    });
      }
    },
    toggleRemainder(id)
    {
      this.tasks = this.tasks.map((task) => task.id === id ? {...task,reminder:!task.reminder} : task)
    },
    async addTask(task)
    {
      this.tasks = [...this.tasks,task]
     let data = {
        "userid":Number(this.userData['id']),
        "text": task.text,
        "day": task.day,
        "reminder": Boolean(task.reminder)
}
      if(this.authToken != "") {
                await fetch("http://127.0.0.1:8000/api/todo-task/", {
                    method: "POST",
                    headers: {
                    Accept: "application/json",
                    "Content-Type": "application/json",
                    'Authorization':'Token ' + this.authToken,
                    },
                    body: JSON.stringify(data)

                })
                    .then((data) => {
                      console.log(data)
                    });

                } else {
                    console.log("Something went wrong. Please try agains");
                }

    },
    toogleTask()
    {
      this.showaddTask = !this.showaddTask
    }
  },
  created()
  {
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
  margin-top: 150px;
}
.login {
  max-width: 500px;
  margin: 50px auto;
  overflow: auto;
  min-height: 500px;
  border: 2px solid green;
  padding: 30px;
  border-radius: 5px;
  margin-top: 150px;
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
