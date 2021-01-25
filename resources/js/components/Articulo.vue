<template>
    <div>
        <h1 class="text-center" >
            Articulos
        </h1>
        <!-- Button trigger modal -->
        <button @click="modificar=false; abrirModal();" type="button" class="btn btn-primary" >
        Agregar Articulo
        </button>

        <!-- Modal -->
        <div class="modal" :class="{mostrar:modal}" >
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">{{  tituloModal }}</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span @click="cerrarModal();" aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <div class="form-group" >
                        <label for="">Articulo</label>
                        <input v-model="articulo.nombre"  class="form-control" type="text" name="nombre" id="nombre" placeholder="nombre" > 

                </div>
                <div class="form-group" >
                        <label for="">descripcion</label>
                        <input v-model="articulo.descripcion"  class="form-control" type="text" name="descripcion" id="descripcion" placeholder="Descripcion del articulo" >      
                </div>

                <div class="form-group" >
                        <label for="">stock</label>
                        <input  v-model="articulo.stock" class="form-control" type="number" name="stock" id="stock" placeholder="stock del articulo" >      
                </div>
                
            </div>
            <div class="modal-footer">
                <button type="button" @click="cerrarModal();" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button @click="guardar();" type="button" class="btn btn-primary">Guardar</button>
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
                        <button @click="modificar=true; abrirModal(art)"  class="btn btn-warning">Editar</button>         
                    </th>
                    <th  > 
                    <button  @click="eliminar(art.id)"  class="btn btn-danger">Eliminar</button>         
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
              articulo:{
                  nombre:"nombre del articulo",
                  descripcion:"descripcion del articulo",
                  stock:1
              },
              modificar:true,
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
          async guardar(id){
            if(this.modificar) {
                const res=await axios.put('/articulos/' +this.id, this.articulo);  
            }else{
                const res=await axios.post('/articulos', this.articulo);                
            }
            this.cerrarModal();
            this.listar();
          },
        abrirModal(data={}) {
                this.modal = 1;
                if(this.modificar){
                    this.id= data.id;
                    this.tituloModal= "Modificar articulo";
                    this.articulo.nombre = data.nombre;
                    this.articulo.descripcion = data.descripcion;
                    this.articulo.stock = data.stock;                    
                }else{
                    this.id=0;
                    this.tituloModal= "Crear articulo";
                    this.articulo.nombre = "";
                    this.articulo.descripcion = "";
                    this.articulo.stock = "";     
                }
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
    