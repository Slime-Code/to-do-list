<template>
  <q-page class="bg-grey-3 column">
  <div class="row q-pa-sm bg-primary">
    <q-input class="col" square v-model="newTask" @keyup.enter="addTask" placeholder="Add task" bg-color="white" dense>
      
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask"  />
        </template>
      </q-input>
  </div>
   <q-list class="bg-white" separator bordered>
      <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->

      <q-item v-ripple  v-for="(task, index) in tasks" :key="task.title" @click="task.done = !task.done" :class="{'done bg-blue-1': task.done}" clickable>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn flat round color="primary" icon="delete" @click.stop="deleteTask(index)" dense/>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
    <q-icon name="check" size="100px" color="primary"/>
      <div class="text-h5 text-primary text-center">
          No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
  import { ref } from 'vue'
  import { useQuasar } from 'quasar'
  export default{
    data(){
      return{
        newTask: '',
        tasks:[
          /*{
            title:'Josué Rufino',
            done: false
          },
          {
            title:'Honório Sousa',
            done: false
          },
          {
            title:'Patricio Wilder',
            done: false
          },
           {
            title:'Yuri Rego',
            done: false
          },
           {
            title:'Laurindo Gibui',
            done: false
          },*/
        ]
      }
    },
    methods:{
      deleteTask(index){
         this.$q.dialog({
        title: 'Confirm',
        message: 'Deseja reseja realmente apagar?',
        cancel: true,
        persistent: true
      }).onOk(() => {
         this.tasks.splice(index, 1)
         this.$q.notify('Apagado com sucesso!')
      })
      },
      addTask(){
        this.tasks.push({
          title: this.newTask,
          done: false
        })
        this.newTask=''
      }
    }
  }
</script>
<style lang="scss">
  .done{
    .q-item__label{
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks{
    opacity: 0.3;
  }
</style>