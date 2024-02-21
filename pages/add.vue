<template>
  
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-text-field
      v-model="username"
      :counter="10"
      label="UserName"
      required
    ></v-text-field>

    <v-text-field v-model="password" label="Password" required></v-text-field>

    <v-select
      v-model="dep"
      :items="items"
      label="DEPARTMENT"
      required
    ></v-select>
    <v-file-input v-model="file" accept="image/*"> </v-file-input>
    <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
    <v-btn color="warning" @click="doSave"> Save </v-btn>
  </v-form>

  <div class="text-center">
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          สถานะการบันทึก
        </v-card-title>

        <v-card-text> บันทึกข้อมูลสำเร็จ </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> ตกลง </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>

  <div class="text-center">
    <v-dialog v-model="dialog_error" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          สถานะการบันทึก
        </v-card-title>

        <v-card-text> บันทึกข้อมูลไม่สำเร็จ </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog_error = false">
            ตกลง
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data: () => ({
    file: [],
    valid: true,
    username: "",
    password: "",
    dep: null,
    dialog: false,
    dialog_error: false,
    items: ["IT", "computer", "electronic", "electircal power"],
  }),

  methods: {
    async doSave() {
      console.log("save data");
      console.log(this.username);
      console.log(this.password);
      console.log(this.dep);
      const formData = new FormData();
      formData.append("file", this.file[0]);
      formData.append("username", this.username);
      formData.append("password", this.password);
      formData.append("dep", this.dep);
      const res = await axios.post("http://localhost:7001/add", formData);
      console.log(data.ok);
      this.username = "";
      this.password = "";
      this.dep = "";
      
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>