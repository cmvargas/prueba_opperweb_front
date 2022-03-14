<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header"  class="text-red-500">default header</slot>
          </div>

          <div class="modal-body">
              <div class="flex items-center justify-center">
                    <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mx-2" @click="deleteResource()">Si</button>
                    <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mx-2" @click="$emit('close')">No</button>
              </div>

          </div>
        </div>
      </div>
    </div>
  </Transition>

  <Transition name="modal">
    <div v-if="showWait" class="modal-mask" >
        <div class="modal-wrapper">
            <div class="modal-container">

            <div class="modal-body">
                <div class="flex items-center justify-center">
                        <h1>Espere...</h1>
                </div>

            </div>
            </div>
        </div>
    </div>
  </Transition>

    <Transition name="modal">
    <div v-if="showError" class="modal-mask" >
        <div class="modal-wrapper">
            <div class="modal-container">

            <div class="modal-body">
                <div class="flex items-center justify-center">
                        <h1>Hubo un error, intentelo mas tarde</h1>
                        <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mx-2" @click="showError=false">Cerrar</button>
                </div>

            </div>
            </div>
        </div>
    </div>
  </Transition>

 <Transition name="modal">
    <div v-if="showDone" class="modal-mask" >
        <div class="modal-wrapper">
            <div class="modal-container">

            <div class="modal-body">
                <div class="flex items-center justify-center">
                        <h1>Se ha eliminado correctamente</h1>
                        <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mx-2" @click="showDone=false">Cerrar</button>
                </div>

            </div>
            </div>
        </div>
    </div>
  </Transition>
  
  
</template>

<script>
import axios from "axios";
export default {
  name: 'ModalDelete',
  props: {
    show: Boolean,
    id: Number,
    title:String
  },
  data(){
      return{
          showWait:false,
          showError:false,
          showDone:false
      }
  },
  methods : {
    deleteResource(){
        this.$emit('close')
        this.showWait = true
            axios.delete(`http://varesdev.xyz/api/posts/${this.id}`).then((response)=>{
                console.log(response);
                if(response.data.status &&  response.data.status=="ok"){
                    this.showWait = false
                    this.showDone=true
                    window.location.href = 'post';
                    //this.$router.push({ name: 'post' })
                }
            })
            .catch(()=>{
                this.showWait = false
                this.showError=true
            })
    }
  }
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  /*color: #42b983;*/
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>