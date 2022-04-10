<template>
  <div class="container" style="max-width: 1500px">
    
    <div class="vh-100" style="background-color: #e2d5de;">
      <div class="container py-5 h-100">
        <div class=" row d-flex justify-content-center align-items-center h-100">
          <div class="col col-xl-10">
            <div class="card" style="border-radius: 15px;">
              <div class="card-body p-5">

                <!-- Heading -->
                <h6 class="mb-3">Xuan Yan Todo List</h6>

                <!-- Input -->
                  <form @submit.prevent="submitTask" >
                    <div class="form-outline flex-fill">
                      <label class="float-start form-label" for="form3">Title</label>
                        <input 
                          type="text"
                          v-model="title"
                          placeholder="Enter Task's Title"
                          class="w-100 form-control" 
                          />
                          <div v-if="titleError" class="err-message">
                             Task's title is required.
                          </div>
                    </div>
              
                    <div class="form-outline flex-fill">
                      <label class="float-start form-label" for="form3">Description</label>
                        <textarea
                          rows="4" 
                          type="text"
                          v-model="description"
                          placeholder="Enter Task's Description"
                          class="w-100 form-control"
                          @keyup.enter="submitTask"
                          />
                          <div v-if="descriptionError" class="err-message">
                            Task's description is required
                          </div>
                    </div>
                      <button 
                        type="submit"  
                        class=" float-end btn btn-primary btn-lg ms-2">{{ editedTask !== undefined ? "Update": "Add" }} Task</button>
                  </form>

                  <!-- To-Do List -->
                  <table class="table table-bordered mt-5">
                    <thead>
                      <tr>
                        <th scope="col">Task</th>
                        <th scope="col" style="width: 120px">Status</th>
                        <th scope="col" >Edit</th>
                        <th scope="col" >Delete</th>
                      </tr>
                    </thead>

                    <tbody>
                      <tr v-for="(task, index) in tasks" :key="index">
                        <td>
                          <input class="form-check-input me-2" type="checkbox" @click="toggleStatus(index)" value="" aria-label="..." />
                          <span :class="{ 'line-through' :task.status === 'finished' }">
                            {{ task.title }}
                          </span>
                        </td>
                        <td>
                          <span
                            class="pointer noselect"
                            :class="{
                              'text-danger':task.status === 'to-do',
                              'text-success':task.status === 'finished',
                            }"
                          >
                            {{ (task.status) }}
                          </span>
                        </td>
                        <td class="text-center">
                          <div @click="editTask(index)">
                            <p class="fa fa-pen pointer"></p>
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
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    name: "TodoPage",
    data() {
      return {
        title: "",
        description: "",
        titleError: false,
        descriptionError: false,
        editedTask: undefined,
        statuses: ["to-do", "finished"],
        /* Status could be: 'to-do' / 'finished' */
        tasks: [
          {
            title: "Jogging in the Hello World.",
            status: "to-do",
            description: "Rolling in the hello-world"
          },
          {
            title: "Eating at the Hello World.",
            status: "to-do",
            description: "Fooding in the hello-world"
          },
        ],
      };
    },
    methods: {
      /**
       * toggle status
      */
      toggleStatus(index) {
        if (this.statuses.indexOf(this.tasks[index].status) === 1) {
          this.tasks[index].status = this.statuses[0];
        } else {
          this.tasks[index].status = this.statuses[1];
        }
      },
      /**
       * Capitalize first character
       */
      capitalizeFirstChar(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
      },
      /**
       * Deletes task by index
       */
      deleteTask(index) {
        if (confirm("Are you sure that you want to remove this task?") == true) {
          this.tasks.splice(index, 1);
        }
      },
      /**
       * Edit task
       */
      editTask(index) {
        this.title = this.tasks[index].title;
        this.description = this.tasks[index].description;
        this.editedTask = index;
      },
      /**
       * Add / Update task
       */
      submitTask() {
        if (this.title.length === 0) {
          this.titleError = true;
          return;
        }

        this.titleError = false;

        if(this.description.length === 0) {
          this.descriptionError = true;
          return;
        }

        this.descriptionError = false;
        
        /* We need to update the task */
        if (this.editedTask != undefined) {
          this.tasks[this.editedTask].title = this.title;
          this.tasks[this.editedTask].description = this.description;
          this.editedTask = undefined;
        } else {
          /* We need to add new task */
          this.tasks.push({
            title: this.title,
            status: "to-do",
            description: this.description
          });
        }
        this.title = "";
        this.description = "";
      }
    },
  };
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}

.err-message {
    float: center;
    color: red;
  }
</style>