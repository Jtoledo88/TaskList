<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Listado de Tareas</div>

                    <div class="card-body">
                        <table class="table table-bordered">
                            <tr>
                                <td colspan="5">
                                    <form name="task-form">
                                        <div class="form-group">
                                            <label class="label">Tarea</label>
                                            <div class="input-group">

                                                <input type="text" class="form-control" placeholder="Ingrese una Tarea" aria-label="Input a Task" aria-describedby="btnGroupAddon" v-model="nuevaTarea">
                                                <div class="input-group-prepend">
                                                    <div class="btn btn-primary" id="btnGroupAddon" @click="agregarTarea">Add</div>
                                                </div>
                                            </div>
                                        </div>
                                    </form>
                                </td>
                            </tr>
                            <tr>
                                <th width="70%">Tarea</th>
                                <th width="20%">Estatus</th>
                                <th colspan="2">Acciones</th>
                            </tr>
                            <tr v-for="tarea in tareas" :key="tarea.id">
                                <td>{{ tarea.task }}</td>
                                <td>{{ (tarea.status == 0 ? "Completada" : "Por Hacer") }}</td>
                                <td>
                                    <button:button class="btn btn-info" v-if="tarea.status == 0" @click="editarTarea(tarea)">Por Hacer</button:button>
                                    <button:button class="btn btn-warning" v-else @click="editarTarea(tarea)">Completada</button:button>
                                </td>
                                <td>
                                    <button:button class="btn btn-danger" v-if="tarea.status == 0" @click="deleteTask(tarea)">Eliminar</button:button>
                                    <button:button class="btn btn-danger disabled" v-else>Eliminar</button:button>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
    <script>
    export default {
        mounted(){
            console.log('componente cargado');
            this.listadoTareas();
            this.agregarTarea();
        },
        data(){
            return{
                mensaje:'Bienvenido',
                nuevaTarea: '',
                tareas: []
            }
        },
        methods: {
            listadoTareas() {
                axios
                    .get("/api/task")
                    .then(resp => {
                        console.log(resp.data.mensaje);
                        this.tareas = resp.data;
                    })
                    .catch(error => {
                        console.log(error);
                   });
            },
            agregarTarea(){
                axios
                .post('/api/task',  {
                    task: this.nuevaTarea,
                    status : 1
                })
                .then(resp => {
                    alert(resp.data.mensaje);
                    console.log(resp.data.mensaje);
                    this.nuevaTarea ='';
                    this.listadoTareas();
                  })
                  .catch(error => {
                        console.log(error);
            });
        },
         editarTarea(tarea){
                axios.put('/api/task/' + tarea.id,
                {
                    task : tarea.task,
                    status : tarea.status == 1 ? 0 : 1
                }).then(resp => {
                    console.log(resp.data.mensaje);
                    alert(resp.data.mensaje);
                    this.listadoTareas();
                }).catch(error => {
                        console.log(error);
            });
        },
    deleteTask(tarea) {
      axios
        .delete("/api/task/" + tarea.id)
        .then(resp => {
          alert(resp.data.mensaje);
          console.log(resp.data.mensaje);
          this.listadoTareas();
        })
        .catch(error => {
          console.log(error);
        });
       },
     }
    }
    </script>
   