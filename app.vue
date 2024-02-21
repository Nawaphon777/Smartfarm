<template>
  <v-card>
    <v-layout>
      
      <v-navigation-drawer expand-on-hover rail style="background-color: #264db9 ; color: rgb(248, 248, 248);font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
        
        <v-list density="compact" nav>
          <v-list-item 
            title="WELCOME"
            subtitle="to smartfarm"
          ></v-list-item><hr>
          <v-list-item link to="/" prepend-icon="mdi-chart-donut-variant" title="Dashboard" value="dasboard"></v-list-item>
          <v-list-item link to="/showreport" prepend-icon="mdi-chart-bar-stacked" title="HistoryStatus" value="Report"></v-list-item>
        </v-list>
      </v-navigation-drawer>
      <v-main style="min-height: 100vh; --v-layout-left: 0px;">

        <NuxtPage />
      </v-main>
    </v-layout>
  </v-card>
</template>
