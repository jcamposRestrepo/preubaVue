<template>
  <div ref="modalEle" class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Agregar Tarea</h5>
          <button class="btn-close" data-bs-dismiss="modal" aria-label="Close" ></button>
        </div>
        <div class="modal-body">
          <add-user @addUser="eventEmitterFromChild"/>
        </div>
      </div>
    </div>
  </div>
  <div class="container">
    <div class="row m-2">
      <div class="col-12 text-start"><h1 class="card-title">Tareas</h1></div>
      <div class="col-12 text-start mb-2">
        <button @click="modalUser" class="btn btn-outline-info"
                data-bs-toggle="modal" data-bs-target="#exampleModal">Agregar Tarea <i class="bi bi-file-earmark-plus-fill"></i></button>
      </div>
      <div class="col-6">
        <input class="form-control" type="text" placeholder="buscar tarea por nombre"
               v-model="nameTask" @keyup="filter()"></div>
      <div class="col-6">
        <select class="form-select" v-model="selected" @change="filterSelect">
          <option value="">Todas</option>
          <option value="completo">completo</option>
          <option value="pendiente">pendiente</option>
          <option value="incompleto">incompleto</option>
        </select>
      </div>
    </div>
    <table class="table table-primary">
      <thead>
      <tr>
        <th scope="col"><i class="bi bi-person-lines-fill"></i> Responsable</th>
        <th scope="col"><i class="bi bi-list-task"></i> Tarea</th>
        <th scope="col"><i class="bi bi-briefcase-fill"></i> Estado</th>
        <th scope="col"><i class="bi bi-arrow-down-up"></i> Cambiar estado</th>
        <th scope="col"> Acciones</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task,index) in allTasks" :key="index">
        <td>{{task.encargado}}</td>
        <td>{{ task.tarea }}</td>
        <td><span :class="{ green: task.estado === 'completo',red: task.estado=== 'incompleto', yellow: task.estado === 'pendiente'}">{{task.estado}}</span></td>
        <td><select class="form-select" v-model="selectedUpdate" @change="updateSelect(index)">
          <option value="" disabled>cambiar estado</option>
          <option value="completo">completo</option>
          <option value="pendiente">pendiente</option>
          <option value="incompleto">incompleto</option>
        </select></td>
        <td>
          <button class="btn btn-outline-danger" @click="deleteUser(index)" data-bs-toggle="modal" data-bs-target="#deleteModal">Eliminar <i class="bi bi-trash3-fill"></i></button>
        </td>
      </tr>
      </tbody>
    </table>
    <div class="row">
      <div class="col-12 text-start">
        <span>Cantidad de tareas: {{countTask ? countTask :tasks.length}}</span>

      </div>
    </div>
  </div>



  <div class="modal fade" id="deleteModal" tabindex="-1" ref="modal1" aria-labelledby="deleteModale" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Confirmación</h5>
          <button class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <h1>¿Está seguro de eliminar esta tarea?</h1>
        </div>
        <div class="modal-footer">
          <button class="btn btn-secondary" data-bs-dismiss="modal" @click="confirmation(false)">NO</button>
          <button class="btn btn-danger" data-bs-dismiss="modal" @click="confirmation(true)">SI</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {ref, onMounted, computed} from "vue";
import addUser from "@/components/addUser.vue";
import index from "vuex";
import { Modal } from 'bootstrap';

export default {

  name: "Tareas",
  computed: {
    index() {
      return index
    }
  },
  components: {
    addUser
  },
  mounted() {
  },
  setup (){
    const tasks = ref([
      {encargado: 'juan lopez', tarea: 'archivar', estado: 'completo'},
      {encargado: 'andres alias', tarea: 'documentar', estado: 'completo'},
      {encargado: 'juan mesa', tarea: 'seleccionar', estado: 'pendiente'},
      {encargado: 'nahomy', tarea: 'reusar', estado: 'incompleto'},
      {encargado: 'juan lopez', tarea: 'analizar', estado: 'pendiente'},
    ])
    const nameTask = ref(null)
    const countTask = ref(null)
    const modal = ref(false)
    const confirmacion = ref(false)
    const indexTask = ref(null)
    const selected = ref("")
    const selectedUpdate = ref("")
    const allTasks = ref(tasks.value)
    let modalEle = ref(null);
    let thisModalObj = null;


    const modalTask = ()=>{
      modal.value = true
    }
    onMounted(() => {
      thisModalObj = new Modal(modalEle.value);
    });

    const eventEmitterFromTask = (task) => {
      tasks.value.push(task)
    }
    const deleteTask = (index) =>{
      indexTask.value = index
      confirmacion.value = true
    }
    const confirmation = (value) =>{
      if (value){
        tasks.value.splice(indexTask.value,1)
        confirmacion.value = false
      }else {
        confirmacion.value = false
      }

    }

    const filter = ()=> {
      const resul = tasks.value.filter((item) => {
        return item.tarea.toLowerCase().match(nameTask.value)
      })
      allTasks.value = resul
      countTask.value = resul.length
    }
    const  filterSelect = ()=> {
      if (selected.value === ""){
        allTasks.value = tasks.value
        countTask.value = tasks.value.length

      }else {
        const resul = tasks.value.filter((item) => {
          return item.estado ===selected.value
        })
        allTasks.value = resul
        countTask.value = resul.length

      }
    }
    const updateSelect = (index)=> {
      tasks.value[index].estado = selectedUpdate.value

     }

    return {
      tasks,eventEmitterFromChild: eventEmitterFromTask,
      modalUser: modalTask,modal,deleteUser: deleteTask,confirmacion,
      confirmation,nameTask,filter,allTasks,selected,filterSelect,updateSelect,
      selectedUpdate,modalEle,countTask
    }
  }
}
</script>

<style>
.green, .yellow, .red{
  padding: 10px;
  border-radius: 5px;

}
.green{
background-color: #5bbd5b;

}
.red{
  background-color: #e34848;
}
.yellow{
  background-color: #e5e56f;
  color: black;

}
</style>
