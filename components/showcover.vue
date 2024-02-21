<template>
    <!-- HEADER===================================================================== -->
    <v-card>
      <v-layout style="background-color: #264db9; min-height: 100vh;">
        <v-main style="height:auto; width: auto;">
          <v-card color="grey-lighten-4" flat height="auto" rounded="0" justify="center">
            <v-toolbar density="compact" style="background-color: #264db9; color: #ffffff; height: 70px; padding-top: 10px;">
              <h2 style="font-size: 30px; margin-left: 80px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
                Show Status</h2>
              <v-spacer></v-spacer>
            </v-toolbar>
          </v-card>
          <!-- HEADER===================================================================== -->
          <div style="--v-layout-left: 0px; ">
            <v-data-table :headers="headers" :items="desserts" class="elevation-1" style=" padding-left: 60px;">
            </v-data-table>
          </div>
        </v-main>
      </v-layout>
    </v-card>
  </template>
  
  <script>
  import { onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  
  export default {
    setup() {
      const router = useRouter();
  
      const checkTokenExists = () => {
        const token = localStorage.getItem('token');
        if (!token) {
          console.log('ไม่มี token ใน localStorage');
          router.push('/login');
        }
      };
  
      onMounted(() => {
        checkTokenExists();
      });
  
      return {
        checkTokenExists
      };
    },
  
    data: () => ({
      dialog: false,
      dialogDelete: false,
      headers: [
        { title: 'Date', key: 'Date' },
        { title: 'Time', key: 'Time' },
        { title: 'PH_Value', key: 'PH_value' },
      ],
      desserts: [],
    }),
  
    created() {
      this.showdata();
    },
  
    methods: {
      async back() {
        this.$router.push("/");
      },
      async showdata() {
        const url = 'http://localhost:7001/listcover';
        const res = await fetch(url);
        const data = await res.json();
        this.desserts = data.datas.reverse();
        console.log('data=>', data.datas);
      },
    },
  };
  </script>
  