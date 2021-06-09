<template>
 <div v-if="form.name.length == 0 && listOfKeys.length == 0">
    <v-btn @click="start()">
      Create New Form
    </v-btn>
  </div>
 
  <div v-else>
  <v-form>
    <v-text-field v-model="form.name" label="Name Your Form">      
    </v-text-field>
    <div v-for="(question, index) in form.questions" :key="index">
    <component :label="question.label" :is="question.type" :items="question.options" v-model="question.answer">
    </component> 
     
    </div>
         <v-btn @click="dialog = !dialog">
              Add Question
          </v-btn>
    <v-dialog v-model="dialog" persistant max-width="600px">
        <new-question @newQuestion="questionAdded" @model="dialog = false">
        </new-question>
      </v-dialog>
      <v-btn @click="saveForm()">
        Save
      </v-btn>
      <v-btn @click="loadList()">
        Load List
      </v-btn>
      <div v-if="listOfKeys.length != 0">
        <v-btn  v-for="(name, index) in listOfKeys" :key="index" @click="loadForm(name)">
            {{name}}
        </v-btn>
      </div>

   </v-form>  
  </div>
  

</template>

<script lang="ts">
  
  import Vue from 'vue'
  import NewQuestion from './NewQuestion.vue'
  import {VTextField,VCheckbox,VDatePicker,VSelect,VCombobox,VFileInput} from 'vuetify/lib'
  export default {
    name: 'FormBuilder',
    components: {VTextField, VCheckbox, NewQuestion,VDatePicker,VSelect,VCombobox,VFileInput},

    data: () => ({
      dialog: false,
      listOfKeys: [],
      list: [],
      form: {name:'',
             questions: []
             }
   
    }),

    methods:{
      questionAdded(question:any){
        console.log(this.form)
        this.form.questions.push(question)
        this.dialog=false

      },
      saveForm(){
      
        
        // window.localStorage.setItem('form', JSON.stringify(this.form))
        this.$axios.post('http://localhost:3000/api/form', this.form, {headers: {'Content-Type': 'application/json'}}).then((res)=>{console.log(res)})
      
      },

      

      loadList(){
          this.$axios.get('http://localhost:3000/api/list').then((res)=>{
           
            this.listOfKeys =  res.data
          })
      },

      loadForm(name){
        this.$axios.post('http://localhost:3000/api/load', {name:name}).then((res)=>{
            this.form = res.data
        })
      },

      start(){
        this.form.name = 'New'
       
      }
    },

    watch:{
      
     
      
    },

    mounted(){
     this.loadList()
  }
  }
</script>
