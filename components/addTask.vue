<template>


    <form @submit.prevent="submitForm()" class="task-form" :class="`${openForm ? '' : 'task-form-closed'}`">
      <div class="task-form-top">
      <div class="task-form-top">
      <v-text-field
        name="title"
        label="Title"
        class="add-task-top"
        v-model="data.title"
      ></v-text-field>
      <v-spacer></v-spacer>
      <v-btn class="date-button" @click.prevent="openDate = !openDate">Due By: {{data.date}}</v-btn>
      </div>
    <v-date-picker v-if="openDate" v-model="data.date" :landscape="true" :reactive="true"></v-date-picker>
      <v-text-field
        name="date"
        label="Description"
        id="id"
        class="add-task-input"
        v-model="data.description"
      ></v-text-field>
      <v-btn type="submit" v-if="!openDate" color="success">SUBMIT</v-btn>
      </div>
      <div @click.prevent="openForm = !openForm" class="close-form">
        <v-icon v-if="openForm" >clear</v-icon>
        <v-icon v-else >add</v-icon>

        </div>
    </form>
</template>

<script>

import axios from 'axios'
export default {
  data() {
    return {
      info: [],
      openDate: false,
      showTodo: true,
      openForm: true,
      data: {
        title: null,
        description: null,
        date: null
      },
    }
  },
  methods: {



    submitForm() {
      // Submit todo
      axios.post('http://leefieldhouse.com/api', {
        title: this.data.title,
        description: this.data.description,
        date: this.data.date
      }).then((dat)=>{
        let info = axios.get('http://todoback.test/api').then(data => {
          this.$parent.info = data.data
          console.log(data)
        })

      }).catch(e => {
        console.log(e);
      })
    },
    // end submit todo
  },
}
</script>
<style>
/* start add task */
  .task-form{
    max-width: 100%;
    max-height: 100%;
    background: #303030;
    border: 0.1rem solid #f1c40f;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 1rem;
    position: relative;
    transition: all 1s;

  }

  .task-form-closed {

    max-height: 0%;
    max-width: 0%;
    transition: all 1s;

  }

  .task-form-closed .task-form-top{
    overflow: hidden;
    visibility: hidden;
  }

  .close-form {
    position: absolute;
    right: -1.5rem;
    top: -1rem;
    background: black;
    width: 3rem;
    height: 3rem;
    border-radius: 100%;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }

  .task-form-top {
    display: flex;
    flex-direction: row;
    width: 80%;
    align-items: center;
    flex-wrap: wrap;

  }

  .add-task-top{
    max-width: 100%;
  }



  .add-task-input{
    width: 80%;
  }
  /* end add task */
</style>
