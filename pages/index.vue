<template>
  <div id="app">

  <div class="wrapper">
    <!-- start add task -->
    <addTask />
    <!-- end add task -->

    <!-- start nav -->
    <div class="nav">
      <div class="border-bottom-1" :class="`${showTodo ? 'active' : 'active-2'} `"></div>
      <div @click.prevent="showTodo = true" class="nav-todo" >
        Todo
      </div>
      <div @click.prevent="showTodo = false" class="nav-complete" >
        Complete
        </div>
    </div>
    <br>
    <!-- end nav -->

    <!-- start show task -->
    <div v-if="showTodo" class="todo">
      <v-hover v-for="inf in info" :key="inf.title">
        <div v-if="!inf.complete"  class="card" :class="`background-${hover ? 12 : 2}`" slot-scope="{hover}" >
          <div class="card-heading">
            <div class="card-title">
              <h4 >{{inf.title}} Due: {{inf.due_by}}</h4>
            </div>
            <div class="card-icons">
              <v-icon @click="markComplete(`${inf.id}`)">check</v-icon>
              <v-icon @click.prevent="deleteTask(`${inf.id}`)">delete</v-icon>
            </div>
          </div>
          <div class="card-description">
            {{inf.description}}
          </div>
        </div>
      </v-hover>
    </div>
    <!-- end show task -->

    <!-- start show completed task -->
    <div class="complete" v-if="!showTodo">
      <v-hover v-for="inf in info" :key="inf.title">
        <div v-if="inf.complete" class="card" :class="`background-${hover ? 12 : 2}`" slot-scope="{hover}" >
          <div class="card-heading">
            <div class="card-title">
              <h4 >{{inf.title}} </h4>
            </div>
            <div class="card-icons">
              <v-icon @click="markNotComplete(`${inf.id}`)">clear</v-icon>
              <v-icon @click.prevent="deleteTask(`${inf.id}`)">delete</v-icon>
            </div>
          </div>
          <div class="card-description">
            {{inf.description}}
          </div>
        </div>
      </v-hover>
    </div>
    <!-- end show completed task -->

  </div>
</div>

</template>

<script>

import axios from 'axios'
import addTask from '@/components/addTask.vue'
export default {
  components: {
    addTask
  },
  data () {
    return {
      info: [],
      showTodo: true,
    }
  },
  methods: {

    // start edit todo
    edit(id){
      console.log(id)
    },
    // end edit todo

    // start mark completed
    markComplete(id){
      axios.post('http://leefieldhouse.com/api/mark-complete', {
        id: id
      }).then(data => {
        this.info.forEach(i => {
          if(i.id == id){
            i.complete = true
          }
        })
      }).catch(e => {
        console.log(e);
      })
    },
    // end mark completed

    // start mark not complete
    markNotComplete(id){
      axios.post('http://leefieldhouse.com/api/mark-notcomplete', {
        id: id
      }).then( data => {
        console.log(data.data)
        this.info.forEach(i => {
          if(i.id == id){
            i.complete = false
          }
        })
      })
    },

    // end mark not complete



    // start delete todo
    deleteTask(id){
      axios.delete('http://leefieldhouse.com/api', {
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

    .wrapper {
      width: 90%!important;
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


  .nav {
    display: flex;
    flex-direction: row;
    width: 100%;
    text-align: center;
    position: relative;
  }

  .nav-todo{
    width: 50%;
    cursor: pointer;

  }

  .border-bottom-1{
    height: 0rem;
    bottom: -0.5rem;
    width: 50%;
    left: 0;
    position: absolute;
    border:#F1C40F 1px solid;
    transition: all 2s;
  }

  .active {
    display: block;
   width: 50%;
   transition: all 2s;
   border:#F1C40F 1px solid;

  }

  .active-2 {
    left: 50%;
    transition: all 2s;
  }


  .nav-complete {
    width: 50%;
    cursor: pointer;
  }


  /* start task card */
  .card{
    width: 100%;
    min-height: 3rem;
    max-height: 100%;
    border: 0.05rem solid #F1C40F;
    border-radius: 1%;
    margin-bottom: 1rem;
  }

  .card-heading {
    width: 100%;
    max-height: 100%;
    border-bottom: 0.05rem solid #f1c40f;
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

  .background-12 {
    background-color: #404040;
    opacity: 50%;
    border: 1px solid #fdce0f
  }

  .card-description {
    padding: 0 1rem;
  }
  /* end task card */

</style>
