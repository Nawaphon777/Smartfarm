<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :sort-by="[{ key: 'calories', order: 'asc' }]"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
        style="background-color: #ECF4D6; "
      >
        <v-toolbar-title style="font-size: 30px; font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif; color: #265073;">User total</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn icon @click="navigateTo('/')"><v-icon style="color: #265073; font-size: 40px; width: 40px; justify-self: start;">mdi-arrow-left-circle</v-icon></v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="10"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.username"
                      label="Username"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="10"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.password"
                      label="Password"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="10"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.dep"
                      label="Department"
                    ></v-text-field>
                  </v-col>
                <!-- <v-col>
                  <v-file-input v-model="file" accept="image/*" ></v-file-input>
                </v-col> -->
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
      <div>
</div>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="editItem(item.raw)"
      >
      mdi-account-edit
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item.raw)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>
<script>
import axios from 'axios';
  export default {
    setup(){
      const checkTokenExists = () => {
        const token = localStorage.getItem('token');
        if (token) {
          console.log('มี token อยู่ใน localStorage');
        } else {
          console.log('ไม่มี token ใน localStorage');
          navigateTo('/login')
        }
      };
  
      onMounted(() => {
        checkTokenExists();
      });
    },
    data: () => ({
      
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          title: 'Username',
          align: 'start',
          sortable: false,
          key: 'username',
        },
        { title: 'Password', key: 'password' },
        { title: 'Department', key: 'dep' },
        // {title: 'Image', key: 'img'},
        { title: 'Actions', key: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        username: '',
        password: '',
        dep: '',
        img: ''
      },
      defaultItem: {
        username: '',
        password: '',
        dep: '',
      },
      
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'เพิ่มข้อมูล' : 'เเก้ไขข้อมูล'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.showdata()
    },

    methods: {
      async back(){
        navigateTo("/")
      },  
    async showdata(){
    const url = 'http://localhost:7001/list';
    const res = await fetch(url);
    const data = await res.json()
    this.desserts = data.datas
    console.log('data=>',data.datas)
      },
      async editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true

      },

      deleteItem (item) {
        this.editedIndex = this.desserts.indexOf(item)    
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      async deleteItemConfirm () {
        console.log('del-to-api',this.editedItem.id)
        // this.desserts.splice(this.editedIndex, 1)
        const url = 'http://localhost:7001/delete?id='+ this.editedItem.id ;
         const res = await fetch(url);
         const data = await res.json()
         setTimeout(() => {
          this.showdata()
          console.log('del sussess')
         }, 1500);
         this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
      async save () {
        if (this.editedIndex > -1) {
          // Object.assign(this.desserts[this.editedIndex], this.editedItem)
          const res = await axios.post('http://localhost:7001/edit',this.editedItem)
          console.log('data=',this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
          const res = await axios.post('http://localhost:7001/add',this.editedItem)
          console.log('data=',this.editedItem)
        }
        setTimeout(() => {
          this.showdata()
          console.log('edit sussess')
         }, 1500);
        this.close()
      },
    },
  }
</script>