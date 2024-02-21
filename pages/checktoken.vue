<template>
  <div style="background-color: #265073; background-size: cover; width:auto;  height: 747px; padding-top: 50px;"> 
    <v-card style="background-color: #ECF4D6; height: 500px; width: 500px; margin: 50px auto; border-radius: 50px; padding: 70px 0px; text-align: center;">
      <h1 style="margin-bottom: 40px; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif; font-size: 35px; color: #265073;">CHECK TOKEN IN WEB PAGE</h1>
      <v-btn @click="check" style="margin: 10px; width: 200px; height: 200px; background-color: #2D9596; color: #ffff; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif; font-size: 35px; border-radius: 20px;">Check</v-btn>
      <v-btn @click="navigateTo('/')" style="margin: 10px; width: 200px; height: 200px; background-color: #2D9596; color: #ffff; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif; font-size: 35px; border-radius: 20px;">Back</v-btn>
    </v-card>
  </div>
  </template>
  
  <script>
  
  export default {
    methods: {
      async check(){
        const token = localStorage.getItem('token');
        if (token) {
          console.log('มี token อยู่ใน localStorage');
          alert('มี token อยู่ใน localStorage')
        } else {
          console.log('ไม่มี token ใน localStorage');
          alert("ไม่มี token อยู่ใน localStorage")
        }
      }
    }
  };
  </script>

  <style>
</style>