<template>
  <div style="background-color: #265073;  width:auto;  height: 750px; padding-top: 50px;"> 
  <div class="form_regis"> 
    <h1 style="color: #265073; font-size: 50px; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;">Register</h1>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <h5 style="text-align: left; margin: 30px 0px 0px 0px; font-size: 20px;color: #265073;">Username</h5>
    <v-text-field
      v-model="username"
      :counter="10"
      required
      style="margin: 0px 0px 20px 0px;
      background-color: #9AD0C2;
      height: 55px;"
    ></v-text-field>
    <h5 style="text-align: left; margin: 0px 0px 0px 0px; font-size: 20px;color: #265073;">Password</h5>
    <v-text-field
      v-model="password"
      :counter="8"
      required
      style="margin: 0px 0px 30px 0px;
      background-color: #9AD0C2;
      height: 55px;"
    ></v-text-field>
    <h5 style="text-align: left; margin: 30px 0px 0px 0px; font-size: 20px;color: #265073; ">Department</h5>
    <v-select
      v-model="dep"
      :items="items"
      required
      style="margin: 0px 0px 30px 0px;
      background-color: #9AD0C2;
      height: 55px;"
    ></v-select>
    <!-- <v-btn
      class="mr-4"
      @click="reset"
      style="margin: 10px 0px; 
      background-color: black;
      color: aliceblue;"
      width="100px"
    >
      Login
    </v-btn> -->
    <v-btn
      style="margin: 10px 0px; 
      background-color: #2D9596;
      color: #ffff;"
      width="100px"
      @click="doSave"
    >
      Save
    </v-btn>
    <br><h4>if you have account click for <a href="/login">login</a></h4>
  </v-form>
</div>

  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
         <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          สถานะการบันทึก
        </v-card-title>

        <v-card-text>
           บันทึกข้อมูลสำเร็จ
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="navigateTo('/login')"
          >
            ตกลง
          </v-btn>

        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>

  <div class="text-center">
    <v-dialog
      v-model="dialog_error"
      width="500"
    >
         <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          สถานะการบันทึก
        </v-card-title>

        <v-card-text>
           บันทึกข้อมูลไม่สำเร็จ
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog_error = false"
          >
            ตกลง
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</div>
</template>
<script>
  export default {
    data: () => ({
      valid: true,
      username: '',
      password: '',   
      dep: '',
      dialog: false,
      dialog_error : false,
      items: [
        'IT',
        'computer',
        'electronic',
        'electircal power',
      ],
    }),

    methods: {
       async doSave () {
        console.log('save data')
        console.log(this.username)
        console.log(this.password)
        console.log(this.dep)
        
        //http://localhost/7001/insert?name=username&passwd=password&dep=dep
        const url = 'http://localhost:7001/insert?name='+ this.username +'&password=' + this.password + '&dep='+ this.dep ;
        const res = await fetch(url);
        const data = await res.json()
        if (data.ok == 1) {
          console.log('save success');
         this.dialog = true
        } else {
          console.log('save fali');
         this.dialog_error = true
        }

      },
      
      // reset () {
      //   this.$refs.form.reset()
      // },
     },
  }
</script>


<style>
    .form_regis{
      margin: 0px auto 0px auto;
      background-color: #ECF4D6;
      height: 600px;
      width: 500px;
      padding: 30px;
      text-align:center;
    border-radius: 50px;
    }
    
</style>