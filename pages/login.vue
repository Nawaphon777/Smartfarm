<template> 
  <div style="background-color: #265073; width:auto;  height: 715px; padding-top: 100px;"> 
  <div class="login-form"> 
      <v-row> 
          <v-col> 
              <div style="height: 460px; width: auto; background-color: #265073; border-radius: 10px;" ></div> 
          </v-col> 
          <v-col> 
              <h2 style="color: hsl(207, 50%, 30%); font-size: 50px; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;">Login</h2> 
              <form> 
              <div class="form-group"> 
                  <h5>Username</h5> 
                  <input type="text" id="username" v-model="formData.username" required > 
              </div> 
              <div class="form-group"> 
                  <h5>Password</h5> 
                  <input type="password" id="password" v-model="formData.password" required > 
              </div> 
              <v-btn @click="login">login</v-btn>
              </form> 
              <h3>if you don't id click <a href="/register"> register</a></h3>
          </v-col> 
      </v-row> 
      
  </div> 
  </div> 
  </template> 
  
  <script>
import axios from 'axios';
import path from 'path';
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
    console.log(res.status.message)
    if(res.data.code == 'success'){
      const token = useCookie('token')
      token.value = res.data.token
      localStorage.setItem('token',res.data.token)
          navigateTo('/')
          console.log('del sussess')
      // this.$router.replace("test")
    }else if(res.data.message == 'User not found'){
      alert("ไม่พบ ชื่อนี้ในฐานข้อมูล")
    }else if(res.data.message == 'Incorrect password'){
      alert("รหัสผ่านไม่ถูกต้อง")
    }else {
      alert("ข้อผิดพลาด")
    }

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
      background-color: #ECF4D6; 
  } 
  .form-group { 
      margin: 10px 0; 
  } 
  
  h5 { 
    text-align: left; 
    margin: 20px 0px 0px 0px; 
    font-size: 20px;
    color: #265073;
  } 
  
  input { 
      width: 350px;
      margin: 0px 0px 0px 0px;
      background-color: #9AD0C2;
      height: 55px;
      padding-left: 20px;
  } 
  
  button { 
      margin: 10px 0px; 
      background-color: #2D9596;
      color: #ffff;
      width: 100px;
      height: 45px;
      border-radius: 5px;
  } 
  
  button:hover { 
      background-color: #0056b3; 
  } 
  
  </style>