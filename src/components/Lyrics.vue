<template>
<transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="Lmodal"
        role="dialog"
        aria-labelledby="modalTitle"
        aria-describedby="modalDescription"
      >
        <header
          class="modal-header"
          id="modalTitle"
        >
          <button
            type="button"
            class="Lbtn-close"
            @click="close"
            aria-label="Close modal"
            >
                x
            </button>
          <h2 >{{name }} - {{title}}</h2>          
        </header>

        <section
          class="modal-body"
          id="modalDescription"
        >
          <slot name="body">            
            <p class="text-center" v-text="letra"></p>
          </slot>
        </section>        
      </div>
    </div>
  </transition>
    
</template>

<script>
export default {
    name:'Lyrics',
    data(){
        return {
            letra: 'Buscando letra',
            title: 'Buscando titulo'
        }
    },
    props:{
        name:{
            type:String
        },
        songs:{
            type: Array
        }
    },
    methods:{
        BuscarLetra(){
            //            
            const url='https://api.lyrics.ovh/v1/'+this.name+'/'+this.songs[0].strTrack
            this.title=this.songs[0].strTrack
            console.log(url)
            fetch(url)
            .then(response=> response.json())
            .then(({lyrics}) => {
                if(lyrics){
                    this.letra= lyrics                                    
                }else{
                    this.letra= 'Letra no encontrada'                                  
                }
                //console.log(lyrics)
                })
            .catch(function(error) {
                console.log('Hubo un problema con la petición Fetch:' + error.message);        
            });
        },
        close() {
        this.$emit('close');
        },
    },
}
</script>
<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .Lmodal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;  
    width: 60%;  
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    position: relative;
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    flex-direction: column;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
    overflow: scroll;    
  }

  .Lbtn-close {
    position: absolute;
    top: 25px;
    right: 1%;
    border: none;
    font-size: 40px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
    z-index: 1;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }

  .modal-fade-enter,
  .modal-fade-leave-to {
    opacity: 0.4;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .1s ease;
  }
</style>