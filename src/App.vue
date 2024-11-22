<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Eventos Corporativos</h1>

      <!-- Sección Tipos de Evento -->
      <section class="card card-section">
        <h2 class="section-title">Tipos de Evento</h2>
        <div class="form-container">
          <form @submit.prevent="crearTipoEvento" class="form">
            <div class="form-group">
              <input v-model="nuevoTipo.nombre" class="form-input" placeholder="Nombre del tipo de evento" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoTipo.descripcion" class="form-input" placeholder="Descripción" required />
            </div>
            <button type="submit" class="btn btn-primary">Crear</button>
          </form>
          <ul class="list">
            <li v-for="tipo in tiposEvento" :key="tipo.idTipoEvento" class="list-item">
              <span>{{ tipo.nombre }} - {{ tipo.descripcion }}</span>
              <button @click="editarTipoEvento(tipo)" class="btn btn-primary">Editar</button>
              <button @click="eliminarTipoEvento(tipo.idTipoEvento)" class="btn btn-danger">Eliminar</button>
            </li>
          </ul>
        </div>
      </section>

      <!-- Sección Eventos Corporativos -->
      <section class="card card-section">
        <h2 class="section-title">Eventos Corporativos</h2>
        <div class="form-container">
          <form @submit.prevent="crearEvento" class="form">
            <div class="form-group">
              <input v-model="nuevoEvento.nombre" class="form-input" placeholder="Nombre del evento" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoEvento.fecha" type="date" class="form-input" required />
            </div>
            <div class="form-group">
              <select v-model="nuevoEvento.idTipoEvento" class="form-select" required>
                <option v-for="tipo in tiposEvento" :key="tipo.idTipoEvento" :value="tipo.idTipoEvento">
                  {{ tipo.nombre }}
                </option>
              </select>
            </div>
            <div class="form-group">
              <input v-model="nuevoEvento.ubicacion" class="form-input" placeholder="Ubicación" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoEvento.descripcion" class="form-input" placeholder="Descripción" required />
            </div>
            <button type="submit" class="btn btn-primary">Crear</button>
          </form>
          <ul class="list">
            <li v-for="evento in eventos" :key="evento.idEvento" class="list-item">
              <span>{{ evento.nombre }} - {{ evento.fecha }}</span>
              <button @click="editarEvento(evento)" class="btn btn-primary">Editar</button>
              <button @click="eliminarEvento(evento.idEvento)" class="btn btn-danger">Eliminar</button>
            </li>
          </ul>
        </div>
      </section>

      <!-- Sección Asistentes a Eventos -->
      <section class="card card-section">
        <h2 class="section-title">Asistentes a Eventos</h2>
        <div class="form-container">
          <form @submit.prevent="crearAsistente" class="form">
            <div class="form-group">
              <select v-model="nuevoAsistente.idEvento" class="form-select" required>
                <option v-for="evento in eventos" :key="evento.idEvento" :value="evento.idEvento">
                  {{ evento.nombre }}
                </option>
              </select>
            </div>
            <div class="form-group">
              <input v-model="nuevoAsistente.nombre" class="form-input" placeholder="Nombre del asistente" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoAsistente.correoElectronico" type="email" class="form-input" placeholder="Correo electrónico" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoAsistente.telefono" class="form-input" placeholder="Teléfono" required />
            </div>
            <div class="form-group">
              <input v-model="nuevoAsistente.idRol" type="number" class="form-input" placeholder="ID del rol" required />
            </div>
            <button type="submit" class="btn btn-primary">Crear</button>
          </form>
          <ul class="list">
            <li v-for="asistente in asistentes" :key="asistente.idAsistente" class="list-item">
              <span>{{ asistente.nombre }} - {{ asistente.correoElectronico }}</span>
              <button @click="editarAsistente(asistente)" class="btn btn-primary">Editar</button>
              <button @click="eliminarAsistente(asistente.idAsistente)" class="btn btn-danger">Eliminar</button>
            </li>
          </ul>
        </div>
      </section>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      baseUrl: "http://localhost:3009", // Definir URL base
      tiposEvento: [],
      eventos: [],
      asistentes: [],
      nuevoTipo: {
        idTipoEvento: null,
        nombre: "",
        descripcion: ""
      },
      nuevoEvento: {
        idEvento: null,
        nombre: "",
        fecha: "",
        idTipoEvento: null,
        ubicacion: "",
        descripcion: ""
      },
      nuevoAsistente: {
        idAsistente: null,
        idEvento: null,
        nombre: "",
        correoElectronico: "",
        telefono: "",
        idRol: null
      }
    };
  },
  methods: {
    async cargarDatos() {
      await this.obtenerTiposEvento();
      await this.obtenerEventos();
      await this.obtenerAsistentes();
    },

    // Tipos de Evento
    async obtenerTiposEvento() {
      try {
        const response = await axios.get(`${this.baseUrl}/tipos-evento`);
        this.tiposEvento = response.data;
      } catch (err) {
        console.error("Error al obtener tipos de evento:", err);
      }
    },
    async crearTipoEvento() {
      try {
        if (this.nuevoTipo.idTipoEvento) {
          await axios.put(`${this.baseUrl}/tipos-evento/${this.nuevoTipo.idTipoEvento}`, this.nuevoTipo);
        } else {
          await axios.post(`${this.baseUrl}/tipos-evento`, this.nuevoTipo);
        }
        this.nuevoTipo = { nombre: "", descripcion: "" };
        this.obtenerTiposEvento();
      } catch (err) {
        console.error("Error al crear o editar tipo de evento:", err);
      }
    },
    async eliminarTipoEvento(id) {
      try {
        await axios.delete(`${this.baseUrl}/tipos-evento/${id}`);
        this.obtenerTiposEvento();
      } catch (err) {
        console.error("Error al eliminar tipo de evento:", err);
      }
    },
    editarTipoEvento(tipo) {
      this.nuevoTipo = { ...tipo };
    },

    // Eventos
    async obtenerEventos() {
      try {
        const response = await axios.get(`${this.baseUrl}/eventos`);
        this.eventos = response.data;
      } catch (err) {
        console.error("Error al obtener eventos:", err);
      }
    },
    async crearEvento() {
      try {
        if (this.nuevoEvento.idEvento) {
          await axios.put(`${this.baseUrl}/eventos/${this.nuevoEvento.idEvento}`, this.nuevoEvento);
        } else {
          await axios.post(`${this.baseUrl}/eventos`, this.nuevoEvento);
        }
        this.nuevoEvento = { nombre: "", fecha: "", idTipoEvento: null, ubicacion: "", descripcion: "" };
        this.obtenerEventos();
      } catch (err) {
        console.error("Error al crear o editar evento:", err);
      }
    },
    async eliminarEvento(id) {
      try {
        await axios.delete(`${this.baseUrl}/eventos/${id}`);
        this.obtenerEventos();
      } catch (err) {
        console.error("Error al eliminar evento:", err);
      }
    },
    editarEvento(evento) {
      this.nuevoEvento = { ...evento };
    },

    // Asistentes
    async obtenerAsistentes() {
      try {
        const response = await axios.get(`${this.baseUrl}/asistentes`);
        this.asistentes = response.data;
      } catch (err) {
        console.error("Error al obtener asistentes:", err);
      }
    },
    async crearAsistente() {
      try {
        if (this.nuevoAsistente.idAsistente) {
          await axios.put(`${this.baseUrl}/asistentes/${this.nuevoAsistente.idAsistente}`, this.nuevoAsistente);
        } else {
          await axios.post(`${this.baseUrl}/asistentes`, this.nuevoAsistente);
        }
        this.nuevoAsistente = { nombre: "", correoElectronico: "", telefono: "", idRol: null };
        this.obtenerAsistentes();
      } catch (err) {
        console.error("Error al crear o editar asistente:", err);
      }
    },
    async eliminarAsistente(id) {
      try {
        await axios.delete(`${this.baseUrl}/asistentes/${id}`);
        this.obtenerAsistentes();
      } catch (err) {
        console.error("Error al eliminar asistente:", err);
      }
    },
    editarAsistente(asistente) {
      this.nuevoAsistente = { ...asistente };
    }
  },
  created() {
    this.cargarDatos();
  }
};
</script>

<style>
/* General */
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f7fa;
  margin: 0;
  padding: 0;
}

.container {
  width: 80%;
  margin: 0 auto;
  padding-top: 30px;
}

/* Títulos */
h1.title {
  text-align: center;
  font-size: 36px;
  margin-bottom: 20px;
  color: #333;
}

h2.section-title {
  font-size: 24px;
  color: #007bff;
  margin-bottom: 15px;
}

/* Card */
.card {
  background: #fff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 30px;
}

.card-section {
  display: flex;
  flex-direction: column;
}

/* Formulario */
.form-container {
  display: flex;
  flex-direction: column;
}

.form {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
  margin-bottom: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-input, .form-select {
  padding: 10px;
  font-size: 14px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.btn {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.btn:hover {
  background-color: #0056b3;
}

.list {
  margin-top: 20px;
}

.list-item {
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 5px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.list-item span {
  font-size: 14px;
  color: #333;
}

.btn-danger {
  background-color: #35dc5f;
}

.btn-danger:hover {
  background-color: #5fc823;
}
</style>
