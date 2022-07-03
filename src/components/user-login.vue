
<template>
    <form @submit.prevent="login()" class="add-form">
    <div class="form-control">
        <h2>SIGNIN</h2>
      <label>Username</label>
      <input required type="text" name="username" v-model="input.username" placeholder="Enter Username" />
    </div>
    <div class="form-control">
      <label>Password</label>
      <input
        required
        type="password"
        name="password"
        v-model="input.password"
        placeholder="Enter Password"
      />
    </div>
    <br>
    <button type="submit" class="btn" >Login</button>
    <br>
    <i v-if="token" class="fa fa-spinner fa-spin" style="font-size:35px;color:green;margin-left: 180px;margin-top: 10px;"></i>

   
  </form>
</template>

<script>
    export default {
        name: 'user-login',
        data() {
            return {
                token:false,
                input: {
                    username: "",
                    password: ""
                }
            }
        },
        components:{
        },
        methods: {
           async login() {
               
                if(this.input.username != "" && this.input.password != "") {
                    this.token = true

                await fetch("http://localhost:8000/api/login/", {
                    method: "POST",
                    headers: {
                    Accept: "application/json",
                    "Content-Type": "application/json",
                    },
                    body: JSON.stringify({username:this.input.username,password:this.input.password}),
                })
                    .then((response) => response.json())
                    .then((data) => {
                        
                        this.$emit("user-cradentials",data)
                        
                    });

                } else {
                    console.log("A username and password must be present");
                }
                
            }
        }
    }
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
  display: row;
}
.form-control h2{
    margin:10% 0% 4% 37%;
    font-family:'Times New Roman', Times, serif;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
.btn {
  color:green;
  margin-left: 160px;
  background: lightgray;
  font-size: large;
 
}

</style>