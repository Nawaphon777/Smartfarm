<template>
  <!-- HEADER ===================================================================== -->
  <v-card>
    <v-layout style="background-color: #264db9; min-height: 100vh;">
      <v-main style="height: auto; width: auto;">
        <!-- Navigation Bar -->
        <v-card color="grey-lighten-4" flat height="auto" rounded="0" justify="center">
          <v-toolbar density="compact"
            style="background-color: #f3f3f3; color: #264db9; height: 70px; padding-top: 10px;">
            <h2 style="font-size: 30px; margin-left: 80px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; ">
              Dashboard</h2>
            <v-spacer></v-spacer>
            <h3 style="margin-right: 20px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; font-size: 40px;">
              {{ currentTime }}</h3>
          </v-toolbar>
        </v-card>
        <!-- END HEADER ===================================================================== -->

        <v-row style="display: flex;">

          <!-- Section 1: Control Buttons -->
          <v-row style="align-items: center;">
            <v-col style="justify-items: center;">
              
              <v-row
              
                style="height: 500px; width: 430px; align-items: center; background-color: #f3f3f3; justify-content: center;  border-radius: 50px; margin: 50px  0px 0px 100px; padding: 10px;">
                <a style="text-align: center; font-size:40px;  font-family: fantasy;color: #1f1183;">pump control</a>
                <!-- Button 2 -->
                
                <v-btn size="300" variant="tonal" :color="lightColor" class="rounded-circle">
                  
                  <v-icon size="60" :icon="isLight2On"></v-icon>
                </v-btn>
        
                <v-col cols="auto">
                  <!-- OPEN Button -->
                  <v-btn
                    style="height: 60px; width: 150px; background-color: #199d12; color: #ffffff; border-radius: 50px;"
                    
                    @click="bntSensor1">OPEN</v-btn>
                </v-col>
                
                <v-col cols="auto">
                  <!-- CLOSE Button -->
                  <v-btn
                    style="height: 60px; width: 150px; background-color: #9d1212; color: #ffffff; border-radius: 50px;"
                    @click="bntSensor2">CLOSE</v-btn>
                </v-col>
              </v-row>
            </v-col>
         
          
            <v-col style="justify-items: center;">
              <v-row
                style="height: 500px; width: 430px; align-items: center; background-color: #f3f3f3; justify-content: center;  border-radius: 50px; margin: 50px  0px 0px 100px; padding: 10px;">
                <a style="text-align: center; font-size:40px;  font-family: fantasy;color: #1f1183;">Solinoi control</a>
                <!-- Button 2 -->
                <v-btn size="300" variant="tonal" :color="lightColor1" class="rounded-circle">
                  <v-icon size="60" :icon="solenoid2On"></v-icon>
                </v-btn>
        
                <v-col cols="auto">
                  <!-- OPEN Button -->
                  <v-btn
                    style="height: 60px; width: 150px; background-color: #199d12; color: #ffffff; border-radius: 50px;"
                    @click="bntSensor3">OPEN</v-btn>
                </v-col>
        
                <v-col cols="auto">
                  <!-- CLOSE Button -->
                  <v-btn
                    style="height: 60px; width: 150px; background-color: #9d1212; color: #ffffff; border-radius: 50px;"
                    @click="bntSensor4">CLOSE</v-btn>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        
        

          <!-- Section 2: pH Value -->
          <v-col style="justify-items: center;">
            <v-row
              style="height: 500px; width: 400px; align-items: top; justify-content: center; margin:40px 50px 40px 0px; padding: 20px;">
              <v-col style="align-items: center; margin-top: 40px ;">
                <div class="circular-frame">
                  <v-card class="pH-value-card" outlined rounded-xl>
                    <a style="text-align: center; font-size:40px; margin: 90px; font-family: fantasy; color: #1f1183;">pH value</a>
                    <!-- pH Value Progress Circular -->
                    <v-progress-circular :rotate="0" :size="300" :width="75" :model-value="value3" color="#1f1183"
                      style="margin: 20px 20px 20px 20px; font-family: fantasy; font-size:40px;">
                      {{ value3 }} pH
                    </v-progress-circular>
                  </v-card>
                </div>
              </v-col>
            </v-row>
          </v-col>
          
          
          
          

        </v-row>

       
         

      </v-main>
    </v-layout>
  </v-card>
</template>

<script>

export default {
  setup() {
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
    useHead({
      script: ["/dist/mqtt/mqtt.min.js"],
    });
  },

  data() {
    return {
      year: 0,
      month: 0,
      day: 0,
      drawer: false,
      rail: true,
      value3: 7.18,
      isLight2On: "mdi-engine-off",
      solenoid2On: "mdi mdi-valve-closed",
      sensor1Value: "0",
      sensor2Value: "0",
      currentTime: "loading",
      lightColor: "grey-darken-4",
      lightColor1: "grey-darken-4",
    };
  },

  beforeMount() {
    this.client = mqtt.connect("wss://broker.hivemq.com:8884/mqtt");
    this.client.on("connect", () => {
      console.log("on client connect");
      this.client.subscribe("pum1");
      this.client.subscribe("pum2");
      this.client.subscribe("solenoid1");
      this.client.subscribe("solenoid2");
      this.client.subscribe("jPH1");
    });
    this.client.on("message", (topic, message) => {
    if (topic === "jPH1") {
      this.value3 = message.toString();
    }
    
    if (topic === "pum1") {
      this.sensor1Value = message.toString();
      console.log("sensor1Value =", this.sensor1Value);
      this.isLight2On = "mdi-engine";
      this.lightColor = "blue lighten-3";
    }
    if (topic === "pum2") {
      this.sensor2Value = message.toString();
      console.log("sensor2Value =", this.sensor2Value);
      this.isLight2On = "mdi-engine-off";
      this.lightColor = "grey-darken-4";
    }
    if (topic === "solenoid1") {
      this.sensor3Value = message.toString();
      this.solenoid2On = "mdi mdi-valve-open";
      this.lightColor1 = "blue lighten-3";
    }
    if (topic === "solenoid2") {
      this.sensor4Value = message.toString();
      this.solenoid2On = "mdi mdi-valve-closed";
      this.lightColor1 = "grey-darken-4";
    }
    });
    
  },

 
  
  methods: {
    getCurrentTime() {
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      const seconds = now.getSeconds().toString().padStart(2, '0');
      const day = now.getDate().toString().padStart(2, '0');
      const month = (now.getMonth() + 1).toString().padStart(2, '0');
      const year = now.getFullYear().toString();
      this.year = year;
      this.month = month;
      this.day = day;
      this.h25 = hours;
      this.h24 = minutes;
      this.h23 = seconds;
      this.currentTime = `${hours}:${minutes}:${seconds}`;
    },
    async token() {
      navigateTo("/checktoken")
    },
    async showdata() {
      navigateTo("/showdata")
    },
    async register() {
      navigateTo("/registerinapp")
    },
    async Logout() {
      localStorage.removeItem('token')
      navigateTo("/login")
    },
    bntSensor1() {
    this.client.publish("pum1", "1"); // Publish message to turn on relay1
      console.log("Relay 1 turned on");
    },
    bntSensor2() {
      this.client.publish("pum2", "0"); // Publish message to turn off relay1
      console.log("Relay 1 turned off");
    },
    bntSensor3() {
      this.client.publish("solenoid1", "1"); // Publish message to turn on solenoid1
      console.log("Solenoid 1 turned on");
    },
    bntSensor4() {
      this.client.publish("solenoid2", "0"); // Publish message to turn off solenoid1
      console.log("Solenoid 1 turned off");
    },
       
    
    async doSave() {
      console.log('save data')
      console.log(this.value)
      console.log(this.value2)
      console.log(this.h25 + ':' + this.h24 + ':' + this.h23)
      console.log(this.year + '-' + this.month + '-' + this.day)
      //http://localhost/7001/insert?name=username&passwd=password&dep=dep
      const url = 'http://localhost:7001/temp?' +'Date=' + this.year + '-' + this.month + '-' + this.day +'&Time=' + this.h25 + ':' + this.h24 + ':' + this.h23 + '&ph=' + this.value3;
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
    setupInterval() {
      this.intervalId = setInterval(() => {
        this.doSave();
      }, 60000);
    },
    clearInterval() {
      clearInterval(this.intervalId);
    },
  },

  mounted() {
    this.getCurrentTime();
    this.intervalId = setInterval(() => {
      this.getCurrentTime();
    }, 1000);
    this.setupInterval();
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  },

};
</script>
  