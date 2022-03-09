<template>
  <div class="container mt-5">
    <router-link to="/crear" class="btn btn-success mb-2">Nuevo <i class="fas fa-plus-circle fa-sm"></i></router-link>
    <div class="card text-center">
      <div class="card-header">Empleados</div>
      <div class="card-body">
        <div class="table-responsive">
          <table class="table table-hover">
            <thead>
              <tr>
                <th scope="col">ID</th>
                <th scope="col">Nombre</th>
                <th scope="col">Correo</th>
                <th scope="col">Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="empleado in empleados" :key="empleado.id">
                <th scope="row">{{ empleado.id }}</th>
                <td>{{ empleado.nombre }}</td>
                <td>{{ empleado.correo }}</td>
                <td>
                  <div
                    class="btn-group"
                    role="group"
                    aria-label="Basic example"
                  >
                    <router-link
                      :to="{ name: 'editar', params: { id: empleado.id } }"
                      class="btn btn-sm btn-outline-primary"
                      ><i class="fas fa-edit fa-lg"></i
                    ></router-link>
                    <button
                      type="button"
                      v-on:click="borrarEmpleado(empleado.id)"
                      class="btn btn-sm btn-outline-danger ms-2"
                    >
                      <i class="fas fa-trash fa-lg"></i>
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      empleados: [],
    };
  },
  created: function () {
    this.consultarEmpleados();
  },
  methods: {
    consultarEmpleados() {
      fetch("http://localhost/empleados/")
        .then((respuesta) => respuesta.json())
        .then((datosRespuesta) => {
          console.log(datosRespuesta);
          this.empleados = [];
          if (typeof datosRespuesta[0].success === "undefined") {
            this.empleados = datosRespuesta;
          }
        })
        .catch(console.log);
    },
    borrarEmpleado(id) {
      console.log(id);
      fetch("http://localhost/empleados/?borrar=" + id)
        .then((respuesta) => respuesta.json())
        .then((datosRespuesta) => {
          console.log(datosRespuesta);
          window.location.href = "listar";
        })
        .catch(console.log);
    },
  },
};
</script>