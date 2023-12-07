<template>
    <div class="background-container">
        <h1> {{ message }}</h1>
        
            <div class="lists-container">
                <h2 :style="{textAlign: 'center', marginTop: '2px', color: 'black'}"> {{ header  }}</h2>
             <ul>
            
             <li  id="lists" v-for="(todo , index) in todos" :key="index" @dblclick="editTodo(index)"> 
                <span v-if="index !== editingIndex"> {{ todo }}</span>
                <input v-else v-model="editedTodo"/>
                <button  :style="{ backgroundColor: 'green'}" v-if="index === editingIndex" @click="saveEdit(index)" @keyup.enter="saveEdit(index)">Save Todo</button>
                <button  :style="{ backgroundColor: 'red'}" v-if="index === editingIndex" @click="cancelEdit(index)" @blur="cancelEdit(index)">Cancel</button>
                    <button  class="inner-button"  :style="{ backgroundColor: 'red'}" v-if="index !== editingIndex" @click="deleteTodo"> x 
                    </button><span> <button class="inner-button"  :style="{ backgroundColor: completedTasks[index] ? 'green' : 'inherit'}"
                     @click="completeTask(index)"> &#x2713;</button></span>
                
              </li>
            
             </ul>
        
            </div>
       <div class="input-button">
            <input v-model="newTodo" @keyup.enter="addTodo" />
            <button :style="{ backgroundColor: 'green'}" @click="addTodo"> add todo</button>
        </div>
   
       <footer>
         <p> {{ closingNote }}</p>
       </footer>
    </div>
</template>




<script>
export default {
    data (){
        return{
            message: 'Todo App',
            header:'To Do Lists',
            closingNote: 'Created by Adeoye Halimat',
            todos: [],
            newTodo : "",
            editingIndex: null,
            editedTodo: "",
            completedTasks:[],
        }
    },
    methods: {
        addTodo(){
            if (this.newTodo.trim() !=="") {
                this.todos.push(this.newTodo);
                this.completedTasks.push(false);
                this.newTodo= '';
            }
        },
        deleteTodo (index){ 
            if (confirm('are you sure you want to remove todo from list'))
            {this.todos.splice(index, 1);
            this.completedTasks.splice(index, 1);
            alert('todo removed succcessfully');
           
        }
            

        },
        editTodo(index) {  
        this.editingIndex = index;
        this.editedTodo = this.todos[index];
        this.completedTasks[index] = !this.completedTasks[index];
        

    
    },
      saveEdit(index) {

        if (this.editedTodo.trim() !== '') {
          this.todos[index] = this.editedTodo;
          this.editingIndex = null;
          this.editedTodo = '';
          
        } else {
          this.todos.splice(index, 1);
          this.completedTasks.splice(index, 1);
          this.editingIndex = null;
          this.editedTodo = '';
        }
      },
      cancelEdit() {
        this.editingIndex = null;
        this.editedTodo = '';
      },
      
      completeTask(index) {
        this.completedTasks[index] = !this.completedTasks[index];
      }
    }
}

</script>


<style>
.lists{
    
    color: black;
}
.inner-button{
    border-radius: 50%;
    height: 20px ; 
    width: 20px; 
    margin-left: 5px;
    margin-top: 5px;
    flex: 1;
    
    

}
.lists-container{
    border: 2px solid black;
    padding:2px;
    height: max-content;
    width: max-content;
    background-color: aliceblue;
    border-radius: 10px;
    text-align: left;
    margin-left: 45%;
    

}
.input-button{
    display: flex;
    flex-direction: row;
    gap: 5px;
    margin-top: 10px;
    justify-content: center;
    
   
}

.background-container{
    min-height: 100%;
    background-image: url('../assets/todoImage.jpeg');
    background-size: cover;
    background-attachment: fixed;
    background-repeat: no-repeat;
    display: flex;
    background-position: center;
    flex-direction: column;
}
</style>