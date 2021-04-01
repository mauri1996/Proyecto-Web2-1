<template>    

  <div class="max-w-4xl flex items-center h-auto lg:h-screen flex-wrap mx-auto my-32 lg:my-0">    
	<!--Main Col-->
	<div id="profile" class="w-full lg:w-3/5 rounded-lg lg:rounded-l-lg lg:rounded-r-none shadow-2xl bg-white opacity-75 mx-6 lg:mx-0">	
		<div class="p-4 md:p-12 text-center lg:text-left">
			<!-- Image for mobile view-->
			<div class="block lg:hidden rounded-full shadow-xl mx-auto -mt-16 h-48 w-48 bg-cover bg-center" :style="{'background-image': 'url('+image+')'}"></div>			            
            <div class="form-inline flex flex-wrap content-between">
			<input class=" font-bold pt-8 lg:pt-0 justify-center mb-0 w-3/4 rounded shadow" v-model="querry" placeholder="Busqueda"/>
            
            <button @click.prevent="newSearch()" class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow w-1/4">
                 Buscar
            </button>
            <span v-if="buscando" >                
                Buscando ....
            </span>

            </div>
			<div class="mx-auto lg:mx-0 w-4/5 pt-3 border-b-2 border-green-500 opacity-25"></div>

			<p class="pt-4 text-base font-bold flex items-center justify-center lg:justify-start"><svg class="h-4 fill-current text-green-700 pr-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9 12H1v6a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-6h-8v2H9v-2zm0-1H0V5c0-1.1.9-2 2-2h4V2a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v1h4a2 2 0 0 1 2 2v6h-9V9H9v2zm3-8V2H8v1h4z"/></svg> Bibliografia</p>			
			<p class="pt-8 text-sm"> {{description | formateo}}</p>


			<div class="mt-6 pb-16 lg:pb-0 w-4/5 lg:w-full mx-auto flex flex-wrap items-center justify-between">
                <Imagen v-for="(ig,key) in imag" :key="key" :url="ig"></Imagen>												
			</div>
			
			<!-- Use https://simpleicons.org/ to find the svg for your preferred product --> 

		</div>

	</div>
	
	<!--Img Col-->
	<div class="w-full lg:w-2/5">
		<!-- Big profile image for side bar (desktop) -->
		<img :src="image" class="rounded-none lg:rounded-lg shadow-2xl hidden lg:block">
		<!-- Image from: http://unsplash.com/photos/MP0IUfwrn0A -->
		
	</div>

</div>
</template>

<script>
import Imagen from './Image.vue'
export default {
  components: { Imagen },
    name:'Info',
    data(){
        return{
            querry: '',
            buscando:false,
            Name:'',
            image:'https://source.unsplash.com/MP0IUfwrn0A',
            description:'',
            imag:[],
            buscandoImg:false
        }
    },
    methods:{
        newSearch(){    
            const token = 'LYMbkAkQjYnzO-mQqGGdDPPhvn2Ii2B575kKwWPKNw0'  
            this.buscando=true      
            const URL = 'https://www.theaudiodb.com/api/v1/json/1/search.php?s='+this.querry                        
            console.log(URL)
            fetch(URL)
            .then(response => response.json())
            .then(({artists}) => {
                this.buscando=false 
                console.log(artists)
                
                if(artists){                     

                    this.image=artists[0].strArtistThumb                    
                    if(artists[0].strBiographyES){
                        this.description=artists[0].strBiographyES
                    }else{
                        this.description=artists[0].strBiographyEN
                    }
                    this.$parent.Url=artists[0].strArtistFanart
                }else{
                    this.image='http://4.bp.blogspot.com/-CuZOfJdrDKY/UYmig8q88yI/AAAAAAAAEZM/bzVtIKPhXVA/s1600/Satoshi-nakamoto.gif'
                    this.description= ''
                    this.$parent.Url='https://img.freepik.com/free-vector/white-abstract-background_23-2148810113.jpg?size=626&ext=jpg&ga=GA1.2.1991903213.1617148800'
                }
                this.buscando=false                
            })
            .catch(function(error) {
                console.log('Hubo un problema con la petición Fetch:' + error.message);        
            });

            const NewUrl = 'https://api.unsplash.com/search/photos?query='+this.querry+'&client_id='+token

            this.buscandoImg=true
            fetch(NewUrl)
            .then(response => response.json())
            .then(({results}) => {         
                this.imag = []
                for (var i= 0; i<4; i++) {                                        
                    this.buscandoImg=false                     
                    this.imag.push(results[i].urls.small)                    
                }

            })
            .catch(function(error) {
                console.log('Hubo un problema con la petición Fetch:' + error.message);        
            });
            

        }
    },
    filters:{
        formateo(value){
            return value.substring(0,300)+ '...'
        }
    },
    mounted(){
        this.querry="Metallica"
        this.newSearch()
    },

}
</script>

