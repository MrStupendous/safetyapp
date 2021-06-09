<template>
    <v-card  v-if="check">
        <v-card-text>
        <v-form>
            <v-text-field v-model="label" label="Title">
                
            </v-text-field>
            <v-select v-model="questionType" :items="questions" label="Question Type">
                Question Type
            </v-select>
            <v-text-field v-if="questionOptions" v-model="optionsList" label="Question Options">
            </v-text-field>
            
            <v-btn @click="addQuestion()">
                Save
            </v-btn>
            <v-btn @click="model()">
                Cancel
            </v-btn>
        </v-form>
        </v-card-text>
    </v-card>
</template>

<script lang="ts">


export default {

data: () => ({

questionType: '',
label: '',
optionsList:'',
options: [],
check: true,

questions: [
    'Numeric Input',
    'Date',
    'Drop Down Menu',
    'Checkbox',
    'Textbox',
    'Multi-Select',
    'Monetary Amount',
    'Picture Upload'
],

conversionMap: new Map<String, String>()

}),

mounted(){
    this.conversionMap.set('Date', 'v-date-picker')
    this.conversionMap.set('Drop Down Menu', 'v-select')
    this.conversionMap.set('Checkbox', 'v-checkbox')
    this.conversionMap.set('Textbox', 'v-text-field')
    this.conversionMap.set('Multi-Select', 'v-combobox') //make this work right
    this.conversionMap.set('Numeric Input', 'v-text-field')
    this.conversionMap.set('Monetary Amount', 'v-text-field')
    this.conversionMap.set('Picture Upload', 'v-file-input')
},

methods:{
    addQuestion(){
        let question
        
        if (this.questionOptions){
            let options = this.optionsList.split(',')
            options = options.map((value) => {return value.trim()})
           
            question = {type: this.conversionMap.get(this.questionType), label: this.label, options: options, answer: {}}
        }
        else{
            question = {type: this.conversionMap.get(this.questionType), label: this.label, answer: {}}
        }
        this.$emit('newQuestion', question)    
        
    },

    model(){
        this.$emit('model')
    }


},

computed:{
    questionOptions(){
       
        if (this.questionType == 'Multi-Select' || this.questionType == 'Checkbox' || this.questionType == 'Drop Down Menu')
        {
            
            return true
        }
        else {
            
            return false
        }
    }
}
    

}
</script>
