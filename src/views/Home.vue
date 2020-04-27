<template>
    <div class="main d-flex align-center">
        <v-content>
            <v-container class="">
                <v-row align="center" justify="center" class="">

                    <v-col cols="12" lg="4" align-self="center">
                        <v-card ref="form" class="px-5 py-6" color="rgba(0, 0, 0, 1)" dark>
                            <div class="display-2 d-flex justify-center pa-5">
                                Search
                            </div>
                            <v-card-text>
                            <v-text-field
                                v-model="term"
                                type="username"
                                name="user"
                                label="Search Book"
                                solo
                            ></v-text-field>
                            </v-card-text>
                            <div class="my-2 d-flex justify-center pb-5">

                                <v-btn large color="primary" @click="search">GO</v-btn>
                            
                            </div> 
                        </v-card>
                    </v-col>

                </v-row>
            </v-container>
        </v-content>
    </div>
</template>

<script>


export default {
  name: 'Home',
  data(){
    return{
      term : "",
    }
  },
    methods: {
      search(){
        
        this.$router.push({ name: 'List', params: {id : '123'}})
      }
    }
}
</script>


<style scoped>
.main{
    height: 100vh;
    background-color: aqua;
    background-image: url('https://images.pexels.com/photos/877971/pexels-photo-877971.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940');
    background-size: cover;   
}
</style>