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
                    <div class="mb-6 ">
                      <div class="grid grid-rows-5 gap-2">
                        <input v-model="title" type="text" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="titulo" required>
                        <input v-model="content" type="text" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Contenido" required>

                        <select v-model="categorySelect" class="form-select appearance-none block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding bg-no-repeat border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none">
                          <option v-for="category in categories" v-bind:value="category.id" v-bind:key="category.id">
                              {{category.nombre}}
                          </option>

                        </select>
                        <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mx-2" @click="createPost()">Crear post</button>
                        <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mx-2" @click="$emit('close')">No</button>
                      </div>
                        
                    </div>
                    <br />


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
                        <h1>Datos guardados correctamente</h1>
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
  name: 'ModalForm',
  props: {
    show: Boolean,
    categories: Array,
  },
  data(){
      return{
          showWait:false,
          showError:false,
          showDone:false,
          title:"",
          content:"",
          categorySelect:0
      }
  },
  methods : {
    createPost(){
        this.$emit('close')
        let senData ={
          titulo:this.title,
          contenido:this.content,
          Categorias_id:this.categorySelect
        }
        this.showWait = true
            axios.post(`http://varesdev.xyz/api/posts`, senData).then((response)=>{
              if(response.data.status && response.data.status=="ok"){
                this.showWait = false
                this.showDone=true
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