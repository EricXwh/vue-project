<template>
  <div class="card text-white bg-primary" id="banner">
    <div class="card-body">
      <div class="row">
        <div class="col"></div>
        <div class="col">
          <i class="fa fa-bars" style="font-size: 18px"></i>
          FRAMEWORKS
        </div>
        <div class="col">
          <button
            type="button"
            class="btn btn-primary float-end shadow btn-sm"
            data-bs-toggle="modal"
            data-bs-target="#exampleModal"
            v-on:click="isUpdate = false"
            @click="clear"
          >
            <i class="fa fa-plus-circle"></i> Add
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- The task table -->
  <table class="table table-hover mt-3">
    <thead>
      <tr class="text-center">
        <td scope="col">Title</td>
        <td scope="col">Description</td>
        <td scope="col">Deadline</td>
        <td scope="col">Priority</td>
        <td scope="col">Is Complete</td>
        <td scope="col">Action</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td class="align-middle">{{ task.tit }}</td>
        <td class="align-middle">{{ task.desc }}</td>
        <td class="align-middle">{{ task.dead }}</td>
        <td class="align-middle">{{ task.prior }}</td>
        <td class="text-center align-middle">
          <div v-if="!task.isHiddenCheck">
            <input
              class="form-check-input"
              v-on:click="task.isHiddenCheck = true"
              type="checkbox"
              value=""
              id="flexCheckDefault"
            />
          </div>
          <div v-if="task.isHiddenCheck">
            <input
              class="form-check-input"
              v-on:click="task.isHiddenCheck = false"
              type="checkbox"
              value=""
              id="flexCheckDefault"
              checked
            />
          </div>
        </td>
        <td class="text-center">
          <div class="btn-group-vertical">
            <button
              v-if="!task.isHiddenCheck"
              v-on:click="isUpdate = true"
              @click="clear"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              class="text-light btn btn-sm btn-block bg-primary"
              @click="getIndex(index)"
            >
              <i class="fa fa-edit text-light"></i> Update
            </button>
            <button
              @click="deleteTask(index)"
              class="text-light btn btn-sm btn-block bg-danger"
            >
              <i class="fa fa-times-circle text-light"></i> Delete
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>

  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div
          class="modal-header bg-primary text-center text-light"
          v-if="!isUpdate"
        >
          <h5 class="modal-title text-light text-center">
            <i class="fa-solid fa-circle-plus"></i> Add Task
          </h5>
        </div>
        <div
          class="modal-header bg-primary text-center text-light"
          v-if="isUpdate"
        >
          <h5 class="modal-title text-light text-center">
            <i class="fa-solid fa-circle-plus"></i> Edit Task
          </h5>
        </div>

        <div class="container mt-3">
          <form action="/action_page.php" class="was-validated">
            <div class="mb-3 mt-3">
              <input
                id="titleinput"
                v-model="title"
                type="text"
                class="form-control"
                placeholder="Title"
                v-if="!isUpdate"
                required
              />
              <div class="valid-feedback">Valid.</div>
              <div class="invalid-feedback">New Title is Required!</div>
            </div>
            <div class="mb-3">
              <input
                type="text"
                v-model="description"
                class="form-control"
                placeholder="Description"
                required
              />
              <div class="valid-feedback">Valid.</div>
              <div class="invalid-feedback">Description is Required!</div>
            </div>

            <div class="mb-3">
              <div class="input-group date">
                <input
                  type="date"
                  class="form-control"
                  id="deadlineinput"
                  placeholder=""
                  v-model="deadline"
                  required
                />
                <div class="invalid-feedback">Deadline is Required!</div>
              </div>
            </div>
          </form>
          <div class="form-check mb-3">
            <div class="mt-2 text-center">
              <div><label for="priority">Priority</label></div>
              <div class="form-check form-check-inline">
                <input
                  v-model="priority"
                  class="form-check-input"
                  type="radio"
                  name="inlineRadioOptions"
                  id="inlineRadio1"
                  value="Low"
                />
                <label class="form-check-label" for="inlineRadio1">Low</label>
              </div>
              <div class="form-check form-check-inline">
                <input
                  v-model="priority"
                  class="form-check-input"
                  type="radio"
                  name="inlineRadioOptions"
                  id="inlineRadio2"
                  value="Med"
                />
                <label class="form-check-label" for="inlineRadio2">Med</label>
              </div>
              <div class="form-check form-check-inline">
                <input
                  v-model="priority"
                  class="form-check-input"
                  type="radio"
                  name="inlineRadioOptions"
                  id="inlineRadio3"
                  value="High"
                />
                <label class="form-check-label" for="inlineRadio3">High</label>
              </div>
            </div>
          </div>

          <div class="modal-footer">
            <button
              @click="addTask"
              type="button"
              class="btn btn-primary"
              v-if="!isUpdate"
            >
              <i class="fa fa-plus-circle"></i> Add
            </button>
            <button
              @click="editTask"
              type="button"
              class="btn btn-primary"
              v-if="isUpdate"
            >
              <i class="fa fa-edit"></i> Edit
            </button>
            <button
              type="button"
              class="btn btn-danger"
              data-bs-dismiss="modal"
            >
              <i class="fa fa-ban"></i> Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  props: {
    msg: String,
  },
  data() {
    return {
      description: '',
      title: '',
      deadline: '',
      priority: '',
      tasks: [],
      isUpdate: false,
    };
  },
  methods: {
    clear() {
      this.description = '';
      this.title = '';
      this.deadline = '';
      this.priority = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      toastr.success('Deleted Succesfully');
    },
    addTask() {
      console.log(document.getElementById('titleinput').classList);
      if (this.title.length === 0) return;
      if (this.description.length === 0) return;
      if (this.priority.length === 0) return;
      document.getElementById('titleinput').style.display = 'block';
      document.getElementById('titleinput').value = '';
      if (this.tasks.length != 0) {
        for (let i = 0; i < this.tasks.length; i++) {
          if (this.title == this.tasks[i].tit) {
            document.getElementById('titleinput').classList.remove('is-valid');
            document.getElementById('titleinput').classList.add('is-invalid');
            return;
          }
        }
      }

      if (this.editedTask != null) {
        this.tasks[this.editedTask].Description = this.description;
        this.tasks[this.editedTask].Deadline = this.deadline;
        this.tasks[this.editedTask].Priority = this.priority;
        this.editedTask = null;
      } else {
        this.tasks.push({
          tit: this.title,
          desc: this.description,
          dead: this.deadline,
          prior: this.priority,
        });
      }
      toastr.success('Added Succesfully');

      this.description = '';
      this.title = '';
      this.deadline = '';
      this.priority = '';
    },
    getIndex(index) {
      this.index = index;
    },
    editTask() {
      if (this.description.length === 0) return;
      if (this.priority.length === 0) return;
      this.tasks[this.index].desc = this.description;
      this.tasks[this.index].dead = this.deadline;
      this.tasks[this.index].prior = this.priority;
      toastr.success('Updated Succesfully');
    },
  },
};
</script>

<style scoped>
#banner {
  position: relative;
  background-color: #1666bf;
  top: 0;
}
</style>
