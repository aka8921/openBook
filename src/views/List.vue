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
          @click="getlist( 1 )"
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
       <v-row justify="center">
        <v-col cols="8">
          <v-container class="max-width">
            <v-pagination
              v-model="pagePosition"
              class="my-4"
              :length="pageCount"
              total-visible = 5
              @input="getlist()"
            ></v-pagination>
          </v-container>
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
        totalItems: 0,
        details: [],
        pagePosition: 1,
    }
},
computed: {
  pageCount(){
    return Math.ceil(this.totalItems/this.limit)
  },
  limit(){
    return 20
  }
},

methods:{
    getlist( reset = 0 ){
        if(reset === 1){this.pagePosition = 1}
        var offs = (this.pagePosition-1)*this.limit
        console.log(this.pagePosition) 
        console.log("searching with offset "+offs)
        this.$axios(
          {
             url : `https://openlibrary.org/search.json`,
             method : 'GET',
            params : {
            q : this.searchTerm,
            limit: this.limit,
            offset: offs,
            },
            })
            .then(response => {
              console.log(response.data)
              this.totalItems = response.data.numFound
              this.details = response.data.docs
            })
            .catch( error => {
              console.log('error')
                console.log(error.response.data)
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
  this.searchTerm = this.$route.params.id
  this.getlist()
}
}
</script>
