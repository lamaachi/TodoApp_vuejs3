<template>
  <div class="container">
    <h1 class="text-center mt-1">Todo App</h1>
    <!-- input  -->
    <div class="d-flex w-50 m-auto">
      <input type="text" name="" ref="task-input" id="task-input" class="form-control rounded-0" v-model="newTask">
      <button @click="addTask()" ref="btn-add" class="btn btn-warning rounded-0">{{ btnText }}</button>
    </div>
    <div class="w-50 m-auto">
      <table class="table table-bordered mt-3">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col"  colspan="2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task,index) in tasks" :key="task"> 
            <th scope="row">
              <span :class="{
                'text-decoration-line-through': task.status =='finished'
              }">  
                {{ task.name}}
              </span>
              </th>
            <td scope="row" class="col-fix-width">
              <span @click="changeStatus(index)" :class="{'text-success':task.status=='in-progress','text-primary':task.status == 'to-do'}" class="action text-capitalize">{{ task.status }}</span>
            </td>
            <td scope="row">
              <div class="action" @click="editTask(index)"><i class="fas fa-pen"></i></div>
            </td>
            <td scope="row">
              <div class="action" @click="remove(index)"><i class="fa fa-trash"></i></div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Todo',
  data(){
    return{
      newTask:'',
      btnText:'Save',
      editedTask:null,
      availableStatus:['to-do','in-progress','finished'],
      tasks:[
        {
          name:'hrml',
          status:'to-do'
        },
        {
          name:'css',
          status:'in-progress'
        }
      ]
    }
  },
  methods:{
    addTask(){
      if(this.newTask){
          if(this.editedTask==null){
            this.tasks.push({
              name:this.newTask,
              status:'to-do'
            })
            this.newTask=''
          }else{
            this.tasks[this.editedTask].name= this.newTask
          }
      }
      else{
        alert('Empty Tasks Not Allowed...')
      }
    },

    editTask(index){  
        this.newTask = this.tasks[index].name
        this.editedTask = index
    },
    changeStatus(index){
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex]
    },
    remove(index){
      if(confirm('Are you sure?')){
        this.tasks.splice(index,1);
      }
    },
  },

  components: {
    //
  }
}
</script>

<style >
  .action:hover{
    cursor: pointer;
    color:red;
  }
  .col-fix-width{
    width: 120px;
  }
</style>
