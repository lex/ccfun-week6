<template>
  <div class="students">
    <section>
      <input type="button" value="Get all students" name="get-all" id="get-all" v-on:click="getAll">
    </section>

    <section>
      <input type="text" name="get-one-id" id="get-one-id" placeholder="ID" v-model="getOneId">
      <input type="button" name="get-one-submit" id="get-one-submit" value="Get student by ID" v-on:click="getById">
    </section>

    <section>
      <input type="text" name="add-id" id="add-id" placeholder="ID" v-model="addId">
      <input type="button" name="add-submit" id="add-submit" value="Add new student" v-on:click="add">
    </section>

    <section>
      <input type="text" name="remove-id" id="remove-id" placeholder="ID" v-model="removeId">
      <input type="button" name="remove-submit" id="remove-submit" value="Remove student" v-on:click="remove">
    </section>

    <section>
      <input type="text" name="mark-done-id" id="mark-done-id" placeholder="ID" v-model="markDoneId">
      <input type="text" name="mark-done-name" id="mark-done-name" placeholder="Task" v-model="markDoneTask">
      <input type="button" name="mark-done-submit" id="mark-done-submit" value="Mark task done" v-on:click="markDone">
    </section>

    <section id="output">
      {{ data }}
    </section>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Students',
  data() {
    return {
      baseUrl:
        'https://wg5w99wdwb.execute-api.eu-west-1.amazonaws.com/production',
      data: '',
      getOneId: '',
      addId: '',
      removeId: '',
      markDoneId: '',
      markDoneTask: '',
    };
  },
  methods: {
    getAll() {
      axios.get(`${this.baseUrl}/get/all`).then(({ data }) => {
        this.data = data;
      });
    },
    getById() {
      axios
        .get(`${this.baseUrl}/get/one?id=${this.getOneId}`)
        .then(({ data }) => {
          this.data = data;
          this.getOneId = '';
        });
    },
    add() {
      const body = { Items: [{ id: this.addId }] };
      axios.post(`${this.baseUrl}/post/add`, body).then(({ data }) => {
        this.data = data;
        this.addId = '';
      });
    },
    remove() {
      const body = { Items: [{ id: this.removeId }] };
      axios.post(`${this.baseUrl}/post/remove`, body).then(({ data }) => {
        this.data = data;
        this.removeId = '';
      });
    },
    markDone() {
      const body = {
        Item: { id: this.markDoneId, [this.markDoneTask]: 'COMPLETED' },
      };
      axios.post(`${this.baseUrl}/post/mark-done`, body).then(({ data }) => {
        this.data = data;
        this.markDoneId = '';
        this.markDoneTask = '';
      });
    },
  },
};
</script>

<style scoped>
.students {
  max-width: 800px;
  margin: 0 auto;
}

section {
  margin: 20px 0;
  padding: 10px;
  border: 1px solid #eee;
}

#output {
  background-color: #eee;
  border-color: #ddd;
  min-height: 100px;
  position: relative;
  padding-top: 25px;
}

#output:before {
  content: 'Output';
  display: inline-block;
  position: absolute;
  background-color: #555;
  color: #fff;
  padding: 3px 6px;
  top: 0;
  left: 0;
  border-bottom-right-radius: 5px;
  font-size: 10px;
  font-family: sans-serif;
}
</style>
