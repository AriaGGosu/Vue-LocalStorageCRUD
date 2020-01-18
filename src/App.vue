<template>
  <div class="container mt-5">
    <h3>{{ titulo }}</h3>
    <input type="text" class="form-control my-3" v-model="newTask" @keyup.enter="addNewTask">
    <button class="btn btn-primary" @click="addNewTask">{{ buttonAdd }}</button>
    <div class="my-3"> 
        <div v-for="(task, index) in taskList" :key="task.id" class="alert" :class="[task.estado ? 'alert-primary' : 'alert-danger']" role="alert">
          <div class="d-flex justify-content-between align-items-center">
            <!-- task -->
            <div>
              {{ index + 1 }} - {{ task.nombre }} - {{ task.estado }}
            </div>
            <!-- buttons -->
            <div class="d-flex justify-content-between">
              <button class="btn btn-primary btn-sm" @click="changeTaskState(index)">Estado</button>
              <button class="btn btn-danger btn-sm ml-3" @click="deleteTask(index)">Delete</button>
            </div>
          </div>  
        </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      titulo: "GYM",
      buttonAdd: "AGREGAR",
      taskList: [],
      newTask: ""
    }
  },
  methods:{
    addNewTask() {
      this.taskList.push({ 
        nombre: this.newTask,
        estado: false
      });
      this.newTask = '';
      this.insertInToLocalStorage(this.taskList)
    },
    changeTaskState(index) {
      this.taskList[index].estado = !this.taskList[index].estado
      this.insertInToLocalStorage(this.taskList)     
    },
    deleteTask(index) {
      this.taskList.splice(index, 1);
      this.insertInToLocalStorage(this.taskList)
    },
    insertInToLocalStorage(value){
      // El método JSON.stringify() convierte un objeto o valor de JavaScript en una cadena de texto JSON
      localStorage.setItem("GYM-localDataBase", JSON.stringify(value))
    },
  },
  created() {
    // El método JSON.parse(), convierte un objeto de tipo JSON en un objeto JavaScript
    let datosDB = JSON.parse(localStorage.getItem("GYM-localDataBase"))
    if(datosDB === null){
      this.taskList = []
    }else{
      this.taskList = datosDB
    }
  }
}
</script>