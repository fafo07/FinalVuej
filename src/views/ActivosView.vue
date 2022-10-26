<template>
    <div class="areas">
        <div class="container-fluid">
            <div class="container">
                <div class="row py-4">
                    <div class="col">
                        <h2 class="text-center">Administracion de Activos</h2>
                    </div>
                    <div class="col">
                        <button @click="irA('Agregar', 0)" class="btn btn-dark">Crear Activo</button>
                    </div>
                </div>
                <div class="row py-2">
                    <div class="col">
                        <div class="row">
                            <h3 class="text-center">Lista de Activos</h3>
                            <div class="col-12">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th scope="col">Id</th>
                                            <th scope="col">Activo</th>
                                            <th scope="col">Marca</th>
                                            <th scope="col">Estado</th>
                                            <th scope="col">Acciones</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(activo, index) of lista_activos">
                                            <td>{{ activo.id }}</td>
                                            <td>{{ activo.Activo }}</td>
                                            <td>{{ activo.Marca }}</td>
                                            <td>{{ activo.Estado }}</td>
                                            <td><button class="btn btn-success"
                                                    @click="irA('Editar', activo.id)">Editar</button> <button
                                                    class="btn btn-danger"
                                                    @click="irA('Eliminar', activo.id)">Eliminar</button></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default ({
    name: "#ActivosView",
    data() {
        return {
            lista_activos: [],
        }
    },
    methods: {
        getActivos() {
            axios
                .get("http://localhost:3333/activo")

                .then(response => {
                    this.lista_activos = response.data
                    console.log(response)
                })
                .catch(e => console.log(e))
        },
        irA(opcion, id_activo) {
            if (opcion == 'Agregar') {
                this.$router.push({ name: 'addactivos' });
            }
            if (opcion == 'Editar') {
                this.$router.push({ name: 'editactivos', params: { id: id_activo} });
            }
            if (opcion == 'Eliminar') 
            {
                if (confirm("Esta seguro de eliminar el Activo?")) {
                    axios({
                        method: "delete",
                        url: "http://localhost:3333/activo/" + id_activo,
                    })
                        .then(response => {
                            this.getActivos();
                            console.log(response);
                        })
                        .catch(e => console.log(e))
                }             
            }
        }
    },
    mounted() {
        this.getActivos()
    },
});
</script>

  
  