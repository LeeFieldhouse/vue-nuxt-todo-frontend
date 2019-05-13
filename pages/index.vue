<template>
  <div id="app">

  <div class="wrapper">
    <form @submit.prevent="submitForm()" class="task-form">
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
    </form>
    <v-hover v-for="inf in info" :key="inf.title">
      <div  class="card" :class="`elevation-${hover ? 12 : 2}`" slot-scope="{hover}" >
        <div class="card-heading">
          <div class="card-title">
            <h4 >{{inf.title}}</h4>
          </div>
          <div class="card-icons">
            <v-icon @click.prevent="edit(`${inf.id}`)">edit</v-icon>
            <v-icon @click.prevent="deleteTask(`${inf.id}`)">delete</v-icon>
          </div>
        </div>
      </div>
    </v-hover>


  </div>
</div>

</template>

<script>


import axios from 'axios'
export default {
  data () {
    return {
      info: [],
      openDate: false,

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
      axios.post('http://todoback.test/api', {
        title: this.data.title,
        description: this.data.description,
        date: this.data.date
      }).then((dat)=>{
        let info = axios.get('http://todoback.test/api').then(data => {
          this.info = data.data
          console.log(data)
        })

      }).catch(e => {
        console.log(e);
      })
    },
    // end submit todo

    // start edit todo
    edit(id){
      console.log(id)
    },
    // end edit todo

    // start delete todo
    deleteTask(id){
      axios.delete('http://todoback.test/api', {
        params: {
          id: id
        }
        }).then(data => {
        console.log(data)
        this.info = this.info.filter(inf => {
          return inf.id != id
        })
      })
    }
    // end delete todo
  },
    mounted () {
      axios.get('http://todoback.test/api').then(todo=>{
      this.info = todo.data
    }).catch(e => {
      console.log(e)
    })


  }

}
</script>

<style>

  @media (max-width: 860px){
    .add-task-top{
      width: 100%;
    }

    .date-button{
      width: 100%;
    }
  }


  #app{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  .wrapper{
    width: 70%;
    max-height: 100%;
    display: flex;
    flex-direction: column;
  }

  .task-form{
    width: 100%;
    max-width: 100%;
    background: #303030;
    border: 0.1rem solid #f1c40f;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 1rem;
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


  /* start task card */
  .card{
    width: 100%;
    height: 15rem;
    border: 0.1rem solid #F1C40F;
    border-radius: 1%;
    margin-bottom: 1rem;
  }

  .card-heading {
    width: 100%;
    max-height: 100%;
    border-bottom: 0.1rem solid #f1c40f;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    padding: 0 1rem;
    box-sizing: border-box;
  }

  .card-title {
    max-width: 85%;
  }

  .card-icons {
    width: 10%;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
  }

  .card-icons i {
    cursor: pointer;
  }

  .elevation-12 {
    background-color: #606060;
    opacity: 50%;
  }
  /* end task card */

</style>
