<template>
    <div class="tasks_container">
        <div class="tasks_content">
            <h1>Listado de Telefonos</h1>
            <!--ul class="tasks_list"-->
                <span v-for="telefono in tasks" :key="telefono.id">
                    <p><b>Telefono:</b> {{ telefono.telefono }}</p>
                    <p><b>Tipo:</b>{{ telefono.tipo }}</p>

                    <hr>
                </span>
            <!--/ul-->
        </div>
    </div>
  <hr>
    <div class="add_task">
      <form v-on:submit.prevent="submitForm">
        <div class="form-group">
          <label for="title">Nombre</label>
          <input type="text" class="form-control" id="nombre" v-model="nombre" />
        </div>
        <div class="form-group">
          <label for="description">Apellido</label>
          <input type="text" class="form-control" id="apellido" v-model="apellido" />
        </div>

        <div class="form-group">
          <label for="title">Cédula</label>
          <input type="text" class="form-control" id="cedula" v-model="cedula" />
        </div>
        <div class="form-group">
          <label for="title">Correo</label>
          <input type="text" class="form-control" id="correo" v-model="correo" />
        </div>
        <div class="form-group">
          <button type="submit">Add Estudiante</button>
        </div>
      </form>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                // tasks
                tasks: [''],
                telefono: '',
                tipo: '',
            }
        },
        methods: {
            async getData() {
                try {
                    // fetch tasks
                    const response = await this.$http.get('http://localhost:8000/api/numerost/');
                    // set the data returned as tasks
                    this.tasks = response.data;
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
            async submitForm() {
              try {
                // Send a POST request to the API
                const response = await this.$http.post(
                  "http://localhost:8000/api/numerost/",
                  {
                    nombre: this.nombre,
                    apellido: this.apellido,
                    cedula: this.cedula,
                    correo: this.correo,
                  }
                );
                // Append the returned data to the tasks array
                this.tasks.push(response.data);
                // Reset the title and description field values.
                this.nombre = "";
                this.apellido = "";
                this.correo = "";
                this.cedula = "";
              } catch (error) {
        // Log the error
              console.log(error);
            }
          },
        },
        created() {
            // Fetch tasks on page load
            this.getData();
        }
    }
</script>