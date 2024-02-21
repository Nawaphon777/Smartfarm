<template> 
<div style="background-image: url('https://img.freepik.com/free-vector/bokeh-defocused-background_23-2148497833.jpg?w=2000'); background-size: cover; width:auto;  height: 715px; padding-top: 100px;"> 
<div class="login-form"> 
    <v-row> 
        <v-col> 
            <div style="height: 460px; width: auto; background-color: brown; border-radius: 10px;" ></div> 
        </v-col> 
        <v-col> 
            <h2>Login</h2> 
            <form @submit.prevent="login"> 
            <div class="form-group"> 
                <label for="username">Username</label> 
                <input type="text" id="username" v-model="formData.username" required style="background-color: aliceblue;"> 
            </div> 
            <div class="form-group"> 
                <label for="password">Password</label> 
                <input type="password" id="password" v-model="formData.password" required style="background-color: aliceblue;"> 
            </div> 
            <button type="submit">Login</button> 
            </form> 
        </v-col> 
    </v-row> 
</div> 
</div> 
</template> 

<script>
import axios from 'axios';
export default {
  data() {
    return {
      formData: {
        username : "",
        password : ""
      }
    };
  },
  
methods: {
   async login() {
    console.log("data =>",this.formData)
    const res = await axios.post("http://localhost:7001/login",this.formData);
    }
  }
};
</script>

<style scoped> 

.login-form { 
    width: 800px; 
    height: 500px; 
    padding: 20px;
    border: 1px solid #ccc; 
    border-radius: 5px; 
    margin: 0 auto; 
    box-shadow: 10px 10px; 
    border: 10px; 
    background-color: #0056b3; 
} 
.form-group { 
    margin: 10px 0; 
} 

label { 
    display: block; 
} 

input { 
    width: 100%; 
    padding: 10px; 
    border: 1px solid #ccc; 
    border-radius: 5px; 
} 

button { 
    background-color: #007bff; 
    color: #fff; 
    border: none; 
    padding: 10px 20px; 
    border-radius: 5px; 
    cursor: pointer; 
} 

button:hover { 
    background-color: #0056b3; 
} 

</style>