<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue TodoApp</h2>
    <!-- inputs -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter your task"
        class="form-contro"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
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
          <th>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </th>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              :class="{
                complete: task.status === 'finished',
                todo: task.status === 'to-do',
                inProgress: task.status === 'in-progress'
              }"
              class="pointer"
              >{{ firstCharUpper(task.status) }}</span
            >
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span id="iconPen" class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span id="icon" class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },

  data() {
    return {
      task: '',
      editedTask: null,
      avaliableStatus: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Steal banana from the store.',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'in-progress'
        }
      ]
    }
  },
  methods: {
    submitTask() {
      if (this.task === 0) return
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }

      this.task = ''
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },
    changeStatus(index) {
      let newIndex = this.avaliableStatus.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.avaliableStatus[newIndex]
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  }
}
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.pointer:hover {
  font-weight: bold;
}
#icon {
  color: red;
}
#iconPen {
  color: blue;
}
.finished {
  text-decoration: line-through;
  color: green;
}
.complete {
  color: green;
}
.todo {
  color: #333;
}
.inProgress {
  color: orange;
}
</style>
