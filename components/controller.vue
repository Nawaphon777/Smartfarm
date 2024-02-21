<template>
     <!-- HEADDER===================================================================== -->
    <v-card>
        <v-layout style="background-color: #265073; min-height: 100vh;">
            <v-main style="height:auto; width: auto;">
                <v-card color="grey-lighten-4" flat height="auto" rounded="0" justify="center">
                    <v-toolbar density="compact"
                        style="background-color: #ECF4D6; color: #265073; height: 70px; padding-top: 10px;">
                        <h2
                            style="font-size: 30px; margin-left: 80px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; ">
                            Control Smart Farm</h2>
                        <v-spacer></v-spacer>
                    </v-toolbar>
                </v-card>
    <!-- HEADDER===================================================================== -->
<v-row style="align-items: center;">
                <v-col style="justify-items: center;">
                    <v-row
                        style="height: 500px; width: 430px; align-items: center; background-color: #ECF4D6; justify-content: center;  border-radius: 50px; margin: 50px  0px 0px 100px; padding: 10px;">
                        <v-btn size="300" variant="tonal" :color="lightColor" class="rounded-circle">
                            <v-icon size="60" :icon="isLight2On"></v-icon>
                        </v-btn>

                        <v-col cols="auto">
                            <v-btn
                                style="height: 60px; width: 150px; background-color: #199d12; color: #ffffff; border-radius: 50px;"
                                @click="bntSensor1">OPEN</v-btn>
                        </v-col>

                        <v-col cols="auto">
                            <v-btn
                                style="height: 60px; width: 150px; background-color: #9d1212; color: #ffffff; border-radius: 50px;"
                                @click="bntSensor2">CLOSE</v-btn>
                        </v-col>
                    </v-row>
                </v-col>
                <v-col style="justify-items: center;">
                    
                </v-col>
</v-row>                
            </v-main>

        </v-layout>
    </v-card>
</template>

<script>

export default {
    setup() {
        useHead({
            script: ["/dist/mqtt/mqtt.min.js"],
        });
    },

    data() {
        return {
            isLight2On: "mdi-engine-off",
            lightColor: "grey-darken-4",
            lightColor1: "grey-darken-4",
            lightColor2: "grey-darken-4",   

            sensor1Value: "0",
            sensor2Value: "0",
            sensor21Value: "0",
            sensor11Value: "0",
        };
    },

    beforeMount() {
        this.client = mqtt.connect("ws://broker.emqx.io:8083/mqtt");
        this.client.on("connect", () => {
            console.log("on client connect");
            this.client.subscribe("pum1");
            this.client.subscribe("pum2");
            
        });
        this.client.on("message", (topic, message) => {
            if (topic === "pum1") {
                this.sensor1Value = message.toString();
                console.log("sensor1Data=", this.sensor1Value);
                this.isLight2On = "mdi-engine";
                this.lightColor = "amber-darken-4";
            }
            if (topic === "pum2") {
                this.sensor2Value = message.toString();
                console.log("sensor2Data=", this.sensor2Value);
                this.isLight2On = "mdi-engine-off";
                this.lightColor = "grey-darken-4";
            }
            

        });
    },


    methods: {
        bntSensor1() {
            this.client.publish("pum1", "1");
            console.log("sensor ON");
        },
        bntSensor2() {
            this.client.publish("pum2", "0");
            console.log("sensor OFF");
        }
    },
}
</script>
  