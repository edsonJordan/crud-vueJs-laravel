<template>
    <div>
        <h1 class="text-center" >
            Articulos
        </h1>
        <!-- Button trigger modal -->
        <button @click="abrirModal();" type="button" class="btn btn-primary" >
        Agregar Articulo
        </button>

        <!-- Modal -->
        <div class="modal fade" :class="{mostrar:modal}" >
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">{{  tituloModal }}</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span @click="cerrarModal();" aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
            </div>
            </div>
        </div>
        </div>

        <table class="table table-striped">
            <thead class="thead-light">
                <tr>
                <th scope="col">#</th>
                <th scope="col">nombre</th>
                <th scope="col">descripcion</th>
                <th scope="col">stock</th>
                <th class="text-center" colspan="2" scope="col">Operaciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="art in articulos" :key="art.id">
                <th scope="row">{{ art.id }}</th>
                <th >{{ art.nombre }}</th>
                <th >{{ art.descripcion }}</th>
                <th >{{ art.stock }}</th>      
                    <th>
                        <button   class="btn btn-warning">Editar</button> 
                    </th>
                    <th  > 
                    <button @click="eliminar(art.id)"  class="btn btn-danger">Eliminar</button>         
                    </th>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
    export default {
      data(){
          return {
              modal:0,
              tituloModal:'',
              articulos: [], 
          }
      },
      methods:{
        async listar(){
              const api = await axios.get('/articulos');
              this.articulos = api.data;
          },
        async eliminar(id){
              const api = await axios.delete('/articulos/'+id);              
                this.listar();
          },
        abrirModal() {
                this.modal = 1;
            },
        cerrarModal(){
                this.modal = 0;
            }          
      },
      created(){
          this.listar();
      },
    }
</script>
    <style>
    .mostrar{
    display: list-item;
    opacity: 1;
    background: rgba(44, 38, 75, 0.0849);
    }
    </style>
    