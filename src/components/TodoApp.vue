<script lang="ts">

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availabelStatus: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: 'Công việc đầu tiên',
          status: 'to-do'
        },
        {
          name: 'Công việc vào thứ 2',
          status: 'in-progress'
        },
        {
          name: 'Công việc vào thứ 3',
          status: 'finished'
        }
      ]
    }
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask == null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null
      }

      this.task = ""
    },

    deleteTask(index: any) {
      this.tasks.splice(index, 1)
    },

    editTask(index: any) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },

    changeStatus(index: any) {
      let newIndex = this.availabelStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) {
        newIndex = 0;
      }
      this.tasks[index].status = this.availabelStatus[newIndex];
    },

    firstCharUpper(str: string) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<template>
  <div class="container">
    <h2 class="text-center mt-5">My Todo App Example {{ msg }}</h2>
    <!-- Input -->
    <div class="d-flex">
      <input type="text" class="form-control" v-model="task" placeholder="Enter Text" @keyup.Enter="submitTask">
      <button class="btn btn-warning rounded-0" @click="submitTask">Submit</button>
    </div>

    <!-- Task Table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{
              'finished': task.status === 'finished',
              'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'in-progress',
              'text-success': task.status === 'finished'
            }">
              {{ task.name }}
            </span>
          </td>
          <td style="width: 120px;">
            <span class="pointer" @click="changeStatus(index)" :class="{
              'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'in-progress',
              'text-success': task.status === 'finished'
            }">
              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td>
            <div class="text-center">
              <span class="fa fa-pen" @click="editTask(index)"></span>
            </div>
          </td>
          <td>
            <div class="text-center">
              <span class="fa fa-trash" @click="deleteTask(index)"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
