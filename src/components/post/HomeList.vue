<template >
    <div >
        <div class="flex items-center justify-center">
            <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded my-5" @click="actionForm()">Crear Post</button>
        </div>
        <div class="flex items-center justify-center">
            
            <br>
            <table class="table-auto">
            <thead>
                <tr>
                    <th>Id</th>
                    <th>Categoria id</th>
                    <th>Titulo</th>
                    <th>Contenido</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="postsList in postsLists" v-bind:key="postsList.id">
                    <td>{{postsList.id}}</td>
                    <td>{{postsList.Categorias_id}}</td>
                    <td>{{postsList.titulo}}</td>
                    <td>{{postsList.contenido}}</td>
                    <td>
                        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mx-2">Modificar</button>
                        <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded" id="show-modal" @click="actionDelete(postsList.id, postsList.titulo)">Eliminar</button>
                    </td>
                </tr>
            </tbody>
            </table>
        </div>
            <Teleport to="body">
                <!-- use the modal component, pass in the prop -->
                <modalDelete :show="showModal" @close="showModal = false" :id="idNow" :title="titleNow">
                <template #header>
                    <h3 class="text-red-500">¿Esta segura que desea eliminar el registro con id {{idNow}} y titulo {{titleNow}}?</h3>
                </template>
                </modalDelete>
            </Teleport>
            <Teleport to="body">
                <!-- use the modal component, pass in the prop -->
                <modalForm :show="showModalForm" @close="showModalForm = false" :categories="categoriesNow">
                <template #header>
                    <h3 class="text-red-500">Crear post</h3>
                </template>
                </modalForm>
            </Teleport>
    </div>
</template>


<script>
import axios from "axios";
import modalDelete from '@/components/post/ModalDelete.vue'
import modalForm from '@/components/post/ModalForm.vue'
export default {
  name: 'HomeList',
  components: {
    modalDelete,
    modalForm
  },
  props: {
  },
  data(){
      return{
        postsLists: [],
        showModal: false,
        showModalForm: false,
        idNow:0,
        titleNow:'',
        contentNow:'',
        categoryNow:0,
        categoriesNow:[]
      }
  },
  mounted () {
      console.log('mounted');
      this.obtenerLista()

  },
  created(){
      console.log('created');
  },
  methods : {
      async obtenerLista(){
            axios.get('http://varesdev.xyz/api/posts').then((response)=>{
                this.postsLists = response.data.data
            })

      },

      actionDelete(id, titulo){
          this.idNow = id
          this.titleNow = titulo
          this.showModal = true
      },

      actionForm(){

        axios.get(`http://varesdev.xyz/api/categorias`)
        .then((response)=>{
            console.log(response);
            this.idNow = 0
            this.titleNow = ''
            this.contentNow='',
            this.categoryNow=0
            this.categoriesNow=response.data.data
            this.showModalForm = true
        })
        .catch((error)=>{
            console.log(error);
        })
      }
  }
}
</script>