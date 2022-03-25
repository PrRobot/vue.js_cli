<template>
  <div class="container">
    <div>
      <button class="btn-create" @click="isOpen = !isOpen">Create</button>
      <Create v-if="isOpen" />
      <Update v-if="isUpdate" :task="currentTask" />
    </div>
    <table>
      <thead>
        <tr>
          <th>Hours</th>
          <th>Date</th>
          <th>Note</th>
          <th>Settings</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td>{{ task.username }}</td>
          <td>{{ task.subject }}</td>
          <td>{{ task.message }}</td>
          <td class="flexed">
            <button @click="updateTask(task)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="edit"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                />
              </svg></button
            ><button @click="removeTask(task.id)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="delete"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                />
              </svg>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Create from "./components/Create.vue";
import axios from "axios";
import Update from "./components/Update.vue";
export default {
  name: "App",
  components: {
    Create,
    Update,
  },

  data() {
    return {
      // for Create component
      isOpen: false,
      // for Update component
      isUpdate: false,
      currentTask: { username: "", subject: "", message: "" },
      tasks: [],
    };
  },
  methods: {
    // get All Tasks
    async fetchData() {
      // return a response
      const res = await axios.get("http://localhost:3000/tasks");
      try {
        // put all tasks in tasks variable
        this.tasks = res.data;
      } catch (err) {
        console.log(err);
      }
    },
    // remove task
    async removeTask(taskId) {
      const res = await axios.delete(`http://localhost:3000/tasks/${taskId}`);
      try {
        // check status code is success
        if (res.status === 200) this.fetchData();
      } catch (err) {
        console.log(err);
      }
    },
    updateTask(task) {
      if (task) {
        // change state to toggle Update Component
        this.isUpdate = !this.isUpdate;
        // set task for update
        this.currentTask = task;
      }
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
table {
  width: 900px;
  border-collapse: collapse;
  margin: 50px auto;
}

/* Zebra striping */
tr:nth-of-type(odd) {
  background: #eee;
}

th {
  background: #3498db;
  color: white;
  font-weight: bold;
}

td,
th {
  padding: 10px;
  border: 1px solid #ccc;
  text-align: left;
  font-size: 18px;
}

@media only screen and (max-width: 760px),
  (min-device-width: 768px) and (max-device-width: 1024px) {
  table {
    width: 100%;
  }

  /* Force table to not be like tables anymore */
  table,
  thead,
  tbody,
  th,
  td,
  tr {
    display: block;
  }

  /* Hide table headers (but not display: none;, for accessibility) */
  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }

  tr {
    border: 1px solid #ccc;
  }

  td {
    /* Behave  like a "row" */
    border: none;
    border-bottom: 1px solid #eee;
    position: relative;
    padding-left: 50%;
  }

  td:before {
    /* Now like a table header */
    position: absolute;
    /* Top/left values mimic padding */
    top: 6px;
    left: 6px;
    width: 45%;
    padding-right: 10px;
    white-space: nowrap;
    /* Label the data */
    content: attr(data-column);

    color: #000;
    font-weight: bold;
  }
}
.btn-create {
  padding: 6px 8px;
  background: rgb(80, 179, 80);
  color: white;
  font-size: 1rem;
  border-radius: 5px;
}
button {
  border: none;
  outline: none;
  background: lightgray;
  box-shadow: 0 2px -5px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  padding: 2px;
  cursor: pointer;
}
svg {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
.flexed {
  display: flex;
  justify-content: space-between;
}
.delete {
  color: red;
}
.edit {
  color: green;
}
</style>