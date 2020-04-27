<template>
    <div class="bg">
      
        <v-row class="d-flex justify-center red">
           
           
        
          
          <v-col
          cols="12"
          md="6" sm="12" lg="6"
          class=" pa-0 ma-0 d-flex align-center justify-center black"
          >
        <v-text-field
        class=" ma-0 "
        background-color="#000"
          hide-details
          prepend-icon="mdi-search"
          single-line
          v-model="searchTerm"
          @keyup="getlist( searchTerm )"
          solo
          flat
          label="Search"
        ></v-text-field>
          </v-col>
          
          <v-col
          cols="12"
          md="6" sm="12" lg="6"
          class="purple pa-0 ma-0 d-flex align-center justify-center"
          >
        <div
          class="fullWidth d-flex justify-center align-center "
          @click="getlist( searchTerm )"
          
        ><span class="overline py-5">Search</span></div>
          </v-col>
          </v-row>
      
      
    <v-container >
         <v-row class="d-flex">
           
           
           
          
          <v-col
          cols="12"
          md="4" sm="6" lg="3"
          v-for="detail in details"
          :key="detail.cover_i"
          >



          
          <!--card starts from here-->

          <v-hover v-slot:default="{ hover }">
          <v-card
          class="mx-auto pa-5 "
          max-width="250"
          outlined
          :elevation="hover ? 16 : 2"
          dark
          
          >
              
              <div class="d-flex flex-column justify-center align-center ">
                
                  <v-img :src="getImgUrl(detail.cover_edition_key)" />
                
                <span class="headline pt-2">
                  {{detail.title}}
                </span>
                <span class="caption pt-2">
                  3 editions
                </span>
                <span class="overline pt-2">
                  
                  <v-chip
                    class="ma-2"
                    color="green"
                    text-color="white"
                  >
                    
                      Read Online
                  
                  </v-chip>
                </span>
                
                <div class="pa-0 ma-0">
              </div>
              </div>
              
        </v-card>
        </v-hover>
        <!--card ends-->




        </v-col>


      </v-row> 


  </v-container>
  </div>
</template>

<script>


export default {
  name: 'List',
data(){
    return{
        searchTerm: "",
        details: []
    }
},

methods:{
    getlist( searchTerm ){
      
        // this.$route.params.id = searchTerm
        // alert(this.$route.params.id )
        this.$axios(
          {
             url : `http://openlibrary.org/search.json`,
             method : 'GET',
            params : {
            q : searchTerm,
            limit: 20,
            offset: 0,
            },
            })
            .then(response => {
              console.log('response')
              this.details = response.data.docs;
              console.log(this.details)
            })
            .catch( error => {
              console.log('error')
                console.log(error);
            })
    },
     getImgUrl(cover) {
    var image = `https://covers.openlibrary/org/b/id/${cover}-L.jpg`
    return image
  }
}
}
</script>
