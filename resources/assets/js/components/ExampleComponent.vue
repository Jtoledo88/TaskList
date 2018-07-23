<template>
   

     <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Listado de Tareas</div>
                    <div class="card-body">
                        <table class="table table-bordered">
                            <tr>
                                <th>#</th>
                                <th>Tarea</th>
                                <th>EStatus</th>
                                <th colspan="2">Acciones</th>
                            </tr>
                            <tr v-for="tarea in tareas" :key="tarea.id">
                                <td> {{ tarea.id }} </td>
                                <td> {{ tarea.task }} </td>
                                <td> {{ tarea.status == 0 ? 'Por Hacer' : 'Completado' }} </td>

                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    

</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
