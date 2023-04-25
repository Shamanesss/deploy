<template>
  <h1>Despliegue con Render</h1>
  <div class="contenedor">
    <img src="@/assets/Oops.gif" alt="" v-if="isError" />
    <img src="@/assets/progress.gif" alt="" v-if="isLoading" />
    <div v-if="!isError && !isLoading">
      <div>
        <InputPersonal @tarea-nueva="agregarTarea" />
        <div class="tareas">
          <div class="tarea" v-for="tarea in tareas" :key="tarea.id">
            <div class="tarea_colores">
              <div v-if="!tarea.editing">
                {{ tarea.tarea }}
                <span
                  class="material-symbols-outlined update"
                  @click="editarTarea(tarea)"
                >
                  draw
                </span>
                <span
                  class="material-symbols-outlined delete"
                  @click="borrarTarea(tarea.id)"
                >
                  delete</span
                >
              </div>
              <div v-else>
                <input
                  class="editar"
                  v-model="tarea.tarea"
                  @keydown.enter="terminarEdicion(tarea)"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import InputPersonal from "@/components/InputPersonal.vue";
import axios from "axios";
import { ref, onMounted } from "vue";

const tareas = ref([]);

const obtenerTareas = async () => {
  let isError = false;
  let isLoading = true;

  try {
    const response = await axios.get(
      "https://pildora-render.onrender.com/tareas"
      //    const response = await axios.get('http://localhost:3000/tareas');
    );
    tareas.value = response.data;
  } catch (error) {
    // isError = true;
    console.log(error);
  }
  isLoading = false;
  return isError, isLoading;
};

onMounted(obtenerTareas);

const editarTarea = (tarea) => {
  tarea.editing = true;
};

const terminarEdicion = async (tarea) => {
  tarea.editing = false;
  try {
    await axios.put(`https://pildora-render.onrender.com/${tarea.id}`, tarea);
  } catch (error) {
    console.log(error);
  }
};

//borrar tarea
const borrarTarea = async (id) => {
  try {
    await axios.delete(`https://pildora-render.onrender.com/tareas/${id}`);
    tareas.value = tareas.value.filter((tarea) => tarea.id !== id);
  } catch (error) {
    console.log(error.value);
  }
};
</script>

<style scoped>
h1 {
  color: black;
}
.contenedor {
  margin: 0 auto;
  background-color: #7a4d8e;
  max-width: 30rem;
  border-radius: 10%;
  margin-bottom: 1rem;
  opacity: 0.9;
}
.tarea_colores {
  background-color: #dee2beb4;
  padding: 0.8rem;
  margin: 0.8rem;
  border-radius: 1%;
  position: relative;
}
.tareas {
  padding: 1rem;
}
.delete {
  position: absolute;
  right: 0.5rem;
  top: 0.6rem;
}
.delete:hover,
.update:hover {
  cursor: pointer;
}
.update {
  position: absolute;
  right: 2rem;
  top: 0.5rem;
}
.editar {
  background-color: #dee2bee0;
  outline: 0;
  border: 0;
}
</style>
