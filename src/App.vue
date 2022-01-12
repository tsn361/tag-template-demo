<template>
  <div id="app">
    <TagTemplate :tags="tags"/>
  </div>
</template>

<script>
import TagTemplate from './components/TagTemplate.vue'

export default {
  name: 'App',
  data(){
    return{
      tags:[ 
        {
          type: "input",
          inputType: "text",
          label: "Product name",
          required: true,
          default:'',
          help:'',
          value:'',
          prefix:'tag.product',
          constraints: {}
        },
        {
          type: "input",
          inputType: "number",
          label: "Product ID",
          required: true,
          default:'',
          help:'only positive number allowed',
          value:'',
          prefix:'tag.id',
          constraints:{}
        },
        {
          type: "bool",
          inputType: "checkbox",
          label: "I accept the terms and use",
          required: true,
          default:true,
          help:'',
          value:'',
          prefix:'tag.terms',
          constraints:{}
        },
        {
          type: "choice",
          inputType: "select",
          label: "Choice",
          required: true,
          default:'',
          help:'',
          value:'',
          prefix:'tag.age',
          constraints:{},
          options:[
            {value: 'Male', name:'Male'},
            {value: 'Female', name:'Female'},
            {value: 'Others', name:'Others'}
          ]
        },
      ]
    }
  },
  components: {
    TagTemplate
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
