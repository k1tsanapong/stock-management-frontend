<template>
  <v-app dark>

    
    
  
    

    <v-bottom-navigation 
    v-model="value" 
    fixed 
    app 
    color="teal"
    grow

    

    >


    <v-btn value="dashboard"
          to="/"
          router
          exact
          

    
    >
      <span>Dashboard</span>

      <v-icon>mdi-file-document-outline</v-icon>
    </v-btn>

    <v-btn value="admin"
          to="/admin"
          router
          exact
          

    
    >
      <span>Admin</span>

      <v-icon>mdi-account-tie</v-icon>
    </v-btn>

    <v-btn value=""
    to="/stock"
    
    
    >
      <span>Stock</span>

      <v-icon>mdi-package-variant-closed</v-icon>
    </v-btn>


    <v-btn value="scan">
      <span>Order</span>

      <v-icon>mdi-barcode-scan</v-icon>
    </v-btn>



    <v-btn value="order">
      <span>Order</span>

      <v-icon>mdi-cart-outline</v-icon>
    </v-btn>

    <v-btn value="singOut<">
      <span>Sing Out</span>

      <v-icon>mdi-microphone-variant</v-icon>
    </v-btn>

  </v-bottom-navigation>


    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    
   



  </v-app>

  

</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      value: 'stock',
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: '404'
    }
  }
}
</script>
