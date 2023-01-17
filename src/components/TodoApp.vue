<template>

  <div class="container" style="max-width: 1000px">
    <h2 class="text-center mt-5">Todo App</h2>

    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Listeye bir şeyler ekle"
        class="w-100 form control"
      />
      <button
        type="button"
        style="border-radius: 0%; font-weight: bold"
        class="btn btn-warning" @click="submitTask"
      >
        Ekle
      </button>
    </div>
    <div class="d-flex mt-5">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col" style="width: 120px">Statüsü</th>
            <th scope="col" class="text-center">Düzenle</th>
            <th scope="col" class="text-center">Sil</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <th><span :class="{'text-decoration-line-through':task.status === 'Bitti'}">

                {{task.name}}
            </span> 
         </th>
            <td>
                    <span class="pointer noselect"
                    @click = "changeStatus(index)"
                    :class="{
                         
                         'text-danger' :task.status === 'Yapılacak',
                         'text-success' :task.status === 'Bitti',
                         'text-warning' :task.status === 'Yapılıyor',
                         

                    }"
                    
                    >
                     {{ capitalizeFirstChar(task.status) }}  


                    </span>

            </td>
            <td class="text-center">
              <div @click="editTask(index)">
                <span class="fa fa-pen pointer"></span>
              </div>
            </td>
            <td class="text-center">
              <div @click="deleteTask(index)">
                <span class="fa fa-trash pointer"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
 
  props: {

     msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["Yapılacak", "Yapılıyor", "Bitti"],

      tasks: [
        {
          name: "Markette Süt Al",
          status: "Yapılıyor",
        },
        {
          name: "Marketten Muz Al",
          status: "Yapılacak",
        },
        {
          name: "Marketten Ekmek Al",
          status: "Bitti",
        },
      ],
    };
  },

  methods: {
        
    capitalizeFirstChar(str) {

        return str.charAt(0).toUpperCase() + str.slice(1);
    },

    changeStatus(index) {

        let newIndex = this.statuses.indexOf(this.tasks[index].status);
        if(++newIndex > 2 ) newIndex = 0 ;
        this.tasks[index].status = this.statuses[newIndex]

    },
    
    deleteTask(index) {

       this.tasks.splice(index, 1);

    },

    editTask(index) {
 
         this.task = this.tasks[index].name;
         this.editedTask = index;

    }, 

    submitTask(){

         if(this.task.length === 0 ) return;

         if(this.editedTask !== null ) {

            this.tasks[this.editedTask].name = this.task;
            this.editedTask=null;
         }else {

              
             this.tasks.push({

                name: this.task, 
                status: "todo",
             });

         }
         this.task = "";

    },

  },
};
</script>

<style>
 
  .container {


    width: 100vw;
    height: 100vh;
  
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

  }
</style>
