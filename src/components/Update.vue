<template>
  <div class="aa" style="background: white">
    <div id="abb" class="container" style="text-align: center">
      <form @submit="updateTask">
        <div class="errors-list">
          <div
            class="error"
            v-for="(error, index) in formErorrs"
            :key="index"
            style="font-size: 14px"
          >
            {{ error }}
          </div>
        </div>
        <div class="form-group">
          <label for="">number of hours</label>
          <input
            type="number"
            name="username"
            id="myText"
            class="form-input"
            v-model="username"
            placeholder="Enter the number of hours worked"
          />
        </div>
        <br />
        <div class="form-group">
          <label for="">business day date</label>
          <input
            type="date"
            name="subject"
            class="form-input"
            v-model="subject"
            onclick="saveDynamicDataToFile();"
            placeholder="Enter today's date"
          />
        </div>
        <br />
        <div class="form-group">
          <label for="">Notes about working hours</label>
          <textarea
            name="message"
            class="form-input"
            v-model="message"
          ></textarea>
        </div>

        <div class="form-group">
          <input
            type="submit"
            class="send-button"
            :value="task !== null ? 'update' : 'Add'"
          />
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Update",
  // task will be updated
  props: ["task"],
  data() {
    return {
      formErorrs: [],
      username: this.task.username,
      subject: this.task.subject,
      message: this.task.message,
      maxChars: 10,
    };
  },
  methods: {
    validateForm() {
      this.formErorrs = []; //Empty Erorr To Start Fresh

      //[1] Check If Username Is Empty
      if (!this.username) {
        this.formErorrs.push("Username Cant Be Empty");
      }

      //[2] Check If Subject Is Empty
      if (!this.subject) {
        this.formErorrs.push("Subject Cant Be Empty");
      }

      //[3] Check If Message Is Empty
      if (!this.message) {
        this.formErorrs.push("Message Cant Be Empty");
      }

      //[4] Check If Username Characters Count
      if (!this.username && this.username.iength > this.maxChars) {
        this.formErorrs.push(
          "username Cant Be More Than" + this.maxChars + "Characters"
        );
      }

      //If No Erorr Return True
      if (!this.formErorrs.length) {
        return true;
      }
    },
    async updateTask(ev) {
      ev.preventDefault();
      // if no error , update task
      // pass Taskid(id) and data that will update
      if (this.validateForm()) {
        const res = await axios.put(
          `http://localhost:3000/tasks/${this.task.id}`,
          {
            username: this.username,
            subject: this.subject,
            message: this.message,
          }
        );
        // check if status is success
        if (res.status === 200) {
          alert("Task is Updated");
        }
      }
    },
  },
};
</script>
