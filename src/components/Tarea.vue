<template>
  <div>
    <h1 class="display-4 text-center">Listado de personas</h1>
    <hr />
    <div class="row">
      <div class="col-lg-8 offset-lg-2">
        <div class="card mt-4">
          <div class="card-body">
            <div class="input-group">
              <input
                type="text"
                v-model="tarea"
                class="form-control form-control-lg"
                placeholder="Agregar nombre "
              />
              <input
                type="text"
                v-model="apellido"
                class="form-control form-control-lg"
                placeholder="Agregar apellido "
              />
              <div class="input-group-append">
                <button
                  v-if="!cambio" v-on:click="agregarTarea"
                  class="btn btn-success btn-lg">
                  Agregar
                </button>
                <button v-else v-on:click=" actualizardatos(tarea,id_persona)" class="btn btn-success btn-lg">Editar Dato</button>
              </div>
              <div>
                <button
                   v-on:click="limpiar"
                  class="btn btn-success btn-lg">
                  limpiar
                </button>
              </div>
            </div>
            <br />
            <div class="text-center">
              <div
                v-if="loading"
                class="spinner-border text-success"
                role="status"
              >
                <span class="sr-only">Loading...</span>
              </div>
            </div>

            <h5 v-if="listTareas.length == 0">No hay estudiantes para realizar</h5>
            <ul v-if="!loading" class="list-group">
              <li
                v-for="(tarea, index) of listTareas"
                :key="index"
                class="list-group-item d-flex justify-content-between"
              >
                <span
                  class="cursor"
                  v-bind:class="{ 'text-success': tarea }" v-on:click="cambio_estado(),editarTarea(tarea)"
                >
                  <i
                    v-bind:class="[
                      tarea.estado ? 'far fa-pencil-square' : 'far fa-pencil',
                    ]"
 
                  ></i>
                </span>
               
                {{ tarea.nombre }}
                {{ tarea.apellido }}
                <span
                  class="text-danger cursor"
                  v-on:click="eliminarTarea(tarea.id_persona)"
                >
                  <i class="fas fa-trash-alt"></i>
                </span>
              </li>
            </ul>
         
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const URL = "https://localhost:44327/api/persona";
const URLID="https://localhost:44327/api/persona/";
export default {
  name: "tarea-item",
  data() {
    return {
      tarea: "",
      apellido:"",
      listTareas: [],
      loading: false,
     // cambio: false,
    };
  },
  methods: {
    agregarTarea() {
      const tarea = {
       
        nombre: this.tarea,
        apellido:this.apellido,
        estado:this.estado,
      };
      /*  this.listTareas.push(tarea); */
      this.loading = true;
      axios.post(URL, tarea)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.obtenerTareas();
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
    
    },
    eliminarTarea(id_persona) {
      /* this.listTareas.splice(index, 1) */
      this.loading = true;
      axios
        .delete(URLID+id_persona)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    editarTarea(tarea) {
        this.id_persona= tarea.id_persona;
        this.tarea= tarea.nombre;
        this.apellido= tarea.apellido;

   
    },
    actualizardatos(tarea, id_persona){
      
      tarea={nombre:this.tarea,apellido:this.apellido}
      this.loading = true;
      axios.put(URLID + id_persona, tarea).then(()=> { 
        this.obtenerTareas()
       this.loading = false
   }).catch(() => this.loading = false);
   
    },
    limpiar(){
     this.tarea='';this.apellido='';
     

  
  },
  cambio_estado(){
    this.cambio=!this.cambio;
  },
    obtenerTareas() {
      this.loading = true;
      axios.get(URL).then((response) => {
        console.log(response);
        this.listTareas = response.data;
        this.loading = false;
      }).catch(() => this.loading = false);
    },
  },
  created: function () {
    this.obtenerTareas();
  },
};
</script>

<style scoped>
.cursor {
  cursor: pointer;
}
</style>