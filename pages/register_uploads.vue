<template>
  <v-file-input v-model="file" accept="image/*" ></v-file-input>
  <v-btn color="primary" @click="upload"> Submit </v-btn>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      file: [],
    };
  },
  methods: {
    async upload() {
      // the file object is not empty
      console.log('this.file=',this.file);
      console.log('this.file[0].name=',this.file[0].name);
      // post file to server
      const formData = new FormData();
      formData.append("file", this.file[0]);
      // for (const file of this.file) {
      //   formData.append("file", file);
      // }
      const res = await axios.post("http://localhost:7001/add", formData);
    },
    async uploads() {
      // the file object is not empty
      console.log(this.file);
      console.log(this.file[0].name);
      // post file to server
      const formData = new FormData();
      formData.append("file", this.file[0]);
      // for (const file of this.file) {
      //   formData.append("file", file);
      // }
      const res = await axios.post("http://localhost:7001/add", formData);
    },
  },
};
</script>