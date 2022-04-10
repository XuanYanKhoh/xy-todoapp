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
                          required/>
                          
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
                          required/>
                          
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

<script setup>
import {ref , onMounted} from 'vue';

const title = ref("");
const description = ref("");
const titleError = ref(false);
const descriptionError = ref(false);
const editedTask = ref(undefined);
const statuses = ref(["to-do", "finished"]);
const tasks = ref([]);

// var title = "";
// var description = "";
// var titleError = false;
// var descriptionError = false;
// var editedTask = undefined;
// const statuses = ["to-do", "finished"];
// var tasks = [];

/**
 * toggle status
*/
const toggleStatus = (index) => {
  if (statuses.value.indexOf(tasks.value[index].status) === 1) {
    tasks.value[index].status = statuses.value[0];
  } else {
    tasks.value[index].status = statuses.value[1];
  }
}

/**
 * Deletes task by index
 */
const deleteTask = (index) => {
  if (confirm("Are you sure that you want to remove this task?") == true) {
    tasks.value.splice(index, 1);
  }
}

/**
 * Edit task
 */
const editTask = (index) => {
  title.value = tasks.value[index].title;
  description.value = tasks.value[index].description;
  editedTask.value = index;
}

/**
 * Add / Update task
 */
const submitTask = () => {
  if (title.value.length === 0) {
    titleError.value = true;
    return;
  }

  titleError.value = false;

  if (description.value.length === 0) {
    descriptionError.value = true;
    return;
  }

  descriptionError.value = false;
  /* update the task */
  if (editedTask.value != undefined) {
    tasks.value[editedTask.value].title = title.value;
    tasks.value[editedTask.value].description = description.value;
    editedTask.value = undefined;
  } else {
    /* add new task */
    tasks.value.push({
      title: title.value,
      status: "to-do",
      description: description.value
    });
  }
  title.value = "";
  description.value = "";
}

onMounted(() => {
  tasks.value = [{
      title: "Jogging in the hello-world",
      status: "to-do",
      description: "Rolling in the hello-world"
    },
    {
      title: "Eating in the hello-world",
      status: "to-do",
      description: "Fooding in the hello-world"
    },
    ];
});
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