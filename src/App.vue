<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div class="table table-bordered mt-5">
          <thead>
          <tr>
            <th>To Do</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(item, i) in todoList" :key="i">
            <td class="align-middle w-75">
              {{item.content}}
            </td>
            <td class="align-middle text-center w-75">
              <button class="btn btn-info btn-sm mx-1" @click="handleEdit(item.id)">
                Edit
              </button>
              <button class="btn btn-danger btn-sm mx-1" @click="handleDelete(item.id)">
                Delete
              </button>
            </td>
          </tr>
          </tbody>
          <tfoot>
          <td class="align-middle text-center w-75">
            <div class="form-group">
              <label for="">
                {{editMode ? "Edit" : "Add"}}
              </label>
              <input type="text" class="form-control" v-model="todoItem.content"/>
            </div>
          </td>
          <td class="align-middle text-center w-25">
            <button class="btn btn-primary btn-sm mx-1" @click="handleToDoItem">
              {{editMode ? "Edit" : "Add"}}
            </button>
            <button v-if="editMode" class="btn btn-danger btn-sm mx-1" @click="handleCancel">
              Cancel
            </button>
          </td>
          </tfoot>
        </div>
      </div>
      </div>
  </div>
</template>


<script>
import axios from "axios";
const todoUrl="http://localhost:3501/todod"
export default { name:"App",
  data() {
    return{
      todoList:[],
      todoItem:{},
      editMode: false
    };
  },
  methods: {
    handleEdit(id) {
      this.editMode = true;
      this.todoItem = this.todoList.find((item)=> item.id == id);
    },
    handleCancel(){
      this.editMode = false;
      this.todoItem="";
    },
    
    async handleToDoItem() {
      const id = this.todoItem.id;
      if (this.editMode){
        //if im in the editmode so im gonna update 
        await axios.put(`${todoUrl}/${id}`, this.todoItem);
        this.editMode = false;
        this.todoItem.content ="";
      } else {
        await axios.post(todoUrl, this.todoItem);
        this.todoItem.content ="";
      }
      axios.get(todoUrl).then(
        (Response) =>(this.todoList = Response.data));
    }, 
  async handleDelete(id) {
      await axios.delete(`${todoUrl}/${id}`);
      axios.get(todoUrl).then((response) => (this.todoList=response.data));

    },
    async created(){
    axios.get(todoUrl).then((Response) => (this.todoList = Response.data ));
  },
     },
 
  
};
</script>


<!--

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>


-->