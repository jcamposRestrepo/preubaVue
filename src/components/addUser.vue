<template>
  <div class="alert alert-danger" role="alert" v-if="valid">
    complete los datos requeridos (*)
  </div>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <div class="input-container">
          <input type="text" id="encargado" required="" v-model="nameTask">
          <label for="encargado" class="label">*Responsable</label>
        </div>
      </div>
      <div class="col-12">
        <div class="input-container">
          <input type="text" id="tarea" required="" v-model="task">
          <label for="tarea" class="label">*Tarea</label>
        </div>
      </div>
      <div class="col-12 text-start">
        <label for="tarea">Tarea</label>
        <select class="form-select" v-model="selected">
          <option value="" >Seleccione</option>
          <option value="completo">Completo</option>
          <option value="pendiente">Pendiente</option>
          <option value="incompleto">Incompleto</option>
        </select>
      </div>
      <div class="col-12 m-4">
        <button class="btn btn-outline-success" @click="Add">Agregar <i class="bi bi-floppy-fill"></i></button>
      </div>
    </div>
  </div>



</template>

<script>
import {ref} from "vue";

export default {
  emits:  ["addUser"],
  setup (props, {emit}){
    const nameTask = ref("")
    const task = ref("")
    const selected = ref("")
    const valid = ref(false)
    const Add = () => {
      if (nameTask.value === "" && task.value === ""){
        valid.value = true
      }else {
        const usersForm = {
          encargado: nameTask.value,
          tarea: task.value,
          estado: selected.value ? selected.value : 'incompleto'
        }
        emit("addUser", usersForm)
        nameTask.value = ""
        task.value = ""
        selected.value = ""
        valid.value = false

      }
    }



    return {nameTask, task,valid,selected, Add}
  }
}
</script>

<style>
.input-container {
  position: relative;
  margin: 10px auto;
}

.input-container input[type="text"] {
  font-size: 20px;
  width: 100%;
  border: none;
  border-bottom: 2px solid #ccc;
  padding: 5px 0;
  background-color: transparent;
  outline: none;
}

.input-container .label {
  position: absolute;
  top: 0;
  left: 0;
  color: #ccc;
  transition: all 0.3s ease;
  pointer-events: none;
}

.input-container input[type="text"]:focus ~ .label,
.input-container input[type="text"]:valid ~ .label {
  top: -20px;
  font-size: 16px;
  color: #333;
}

.input-container .underline {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 2px;
  width: 100%;
  background-color: #333;
  transform: scaleX(0);
  transition: all 0.3s ease;
}

.input-container input[type="text"]:focus ~ .underline,
.input-container input[type="text"]:valid ~ .underline {
  transform: scaleX(1);
}

</style>
