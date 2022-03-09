<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header text-center">Editar empleado</div>
      <div class="card-body">
        <form v-on:submit.prevent="actualizarRegistro">
          <div class="mb-3">
            <label for="nombre" class="form-label">Nombre:</label>
            <input
              type="text"
              class="form-control"
              id="nombre"
              aria-describedby="nombreHelp"
              v-model="empleado.nombre"
            />
            <div id="correoHelp" class="form-text">
              Escribe el nombre del empleado
            </div>
          </div>
          <div class="mb-3">
            <label for="correo" class="form-label">Correo:</label>
            <input
              type="email"
              class="form-control"
              id="correo"
              aria-describedby="correoHelp"
              v-model="empleado.correo"
            />
            <div id="correoHelp" class="form-text">
              Escribe el correo del empleado
            </div>
          </div>
          <button type="submit" class="btn btn-success">Actualizar</button>
          <router-link :to="{ name: 'listar' }" class="btn btn-secondary ms-2"
            >Cancelar</router-link
          >
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      empleado: {},
    };
  },
  created: function () {
    this.obtenerInformacionID();
  },
  methods: {
    obtenerInformacionID() {
      fetch("http://localhost/empleados/?consultar=" + this.$route.params.id)
        .then((respuesta) => respuesta.json())
        .then((datosRespuesta) => {
          console.log(datosRespuesta);
          this.empleado = datosRespuesta[0];
        })
        .catch(console.log);
    },
    actualizarRegistro() {
      var datosEnviar = {
        id: this.$route.params.id,
        nombre: this.empleado.nombre,
        correo: this.empleado.correo,
      };
      fetch("http://localhost/empleados/?actualizar=" + this.$route.params.id, {
        method: "POST",
        body: JSON.stringify(datosEnviar),
      })
        .then((respuesta) => respuesta.json())
        .then((datosRespuesta) => {
          console.log(datosRespuesta);
          window.location.href = "../listar";
        });
    },
  },
};
</script>