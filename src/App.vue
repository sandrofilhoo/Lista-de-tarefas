<template>
  <div id="app">
    <h1>Tarefa</h1>
    <BarProgress :progress="progress"/>
    <NewTask @taskAdded="addTask" />
    <TasksGrid @taskDeleted="deleteTask" @taskStateChanged="toggleTaskState" :tasks="tasks"/>
    
  </div>
</template>

<script>
import TasksGrid from './components/TasksGrid.vue'
import NewTask from './components/New-Task.vue'
import BarProgress from './components/BarProgress'

export default {
  name: 'App',
  components: {
    NewTask,
    BarProgress,
    TasksGrid
  },
  computed:{
    progress(){
      const total = this.tasks.length
      const done = this.tasks.filter(t => !t.pending).length
      return Math.round(done / total * 100) || 0
    }
  },
  data(){
    return{
      tasks:[
        { name:"Lavar a louça", pending:false},
        { name:"compar", pending:true}
      ]
    }
  },
  methods:{
    addTask(task){
      const sameName = t => t.name === task.name
      const reallyNew = this.tasks.filter(sameName).length == 0
      if(reallyNew){
        this.tasks.push({
          name: task.name,
          pending: task.pending || true
        })
      }
    },
    deleteTask(i){
      this.tasks.splice(i,1)
    },
    toggleTaskState(i){
      this.tasks[i].pending = !this.tasks[i].pending
    }
  }
}
</script>

<style>
body{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #FFF;
  background: linear-gradient(to right,rgb(22, 34, 42), rgb(58, 96, 115)); 
}
#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #2c3e50;
  height: 100vh;
}
#app h1{
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
  color: #FFF;
}
</style>
