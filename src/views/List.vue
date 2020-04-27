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
          
          solo
          flat
          label="Search"
        ></v-text-field>
          </v-col>
          
          <v-col
          cols="12"
          md="6" sm="12" lg="6"
          class="purple pa-0 ma-0 d-flex align-center justify-center"
          @click="getlist( searchTerm )"
          >
        <div
          class="fullWidth d-flex justify-center align-center "
          
          
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
                
                  <v-img :src="getImgUrl(detail.cover_i)" class="blue-grey darken-4" width = "100%" height="300" alt = "image not found"/>
                
                <span class="headline pt-2">
                  {{detail.title}}
                </span>
                <span class="caption pt-2">
                  {{detail.edition_count}} editions
                </span>
                <span class="overline pt-2">
                  

                  <v-btn :href=readOnline(detail.availability.identifier) target="_blank" :disabled=!(detail.availability.identifier||0)>
                  
                    
                      read online
                  
                  
                  </v-btn>
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
    getlist( searchTerm, ofs = 0 ){
      
        console.log("searching")
        this.$axios(
          {
             url : `https://openlibrary.org/search.json`,
             method : 'GET',
            params : {
            q : searchTerm,
            limit: 20,
            offset: ofs,
            },
            })
            .then(response => {
              console.log('response')
              this.details = response.data.docs;
              console.log(this.details)
            })
            .catch( error => {
              console.log('error')
                console.log(error.response.data);
            })
    },
     getImgUrl(cover) {
    var image = `https://covers.openlibrary.org/b/id/${cover}-L.jpg`
    return image
    },

    readOnline(identifier) {
    var link = `https://archive.org/stream/${identifier}`
    return link
  }
},
created(){
  console.log(this.$route.params.id)
  this.getlist( this.$route.params.id )
}
}
</script>
