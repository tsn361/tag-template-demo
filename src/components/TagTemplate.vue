<template>
  <div>
    <h1>Tag template - demo</h1>
    <b-container>
        <b-row v-for="(tag,key) in tags" :key="key">
          <b-col offset-sm="3">
            <b-row class="my-4" v-if="tag.type === 'input'">
              <b-col sm="2">
                <div class="text-end">
                  <label class="label" :for="`type-${tag.label}`">{{tag.label}}:</label>
                </div>
              </b-col>
              <b-col sm="4" class="text-start">
                  <ValidationProvider mode="eager" :rules="`required: ${tag.required}|alpha`" v-slot="{ errors }">
                    <input :type="tag.inputType" v-model="tag.value" :name="tag.label" :required="tag.required" :value="tag.default" class="form-control" @blur="emitEvent(tag)">
                    <small v-if="tag.help"  class="text-muted">{{tag.help}}</small>
                    <p class="text-danger">{{ errors[0] }}</p>
                </ValidationProvider>
              </b-col>
            </b-row>

            <b-row class="my-4" v-if="tag.type === 'bool'">
              <b-col sm="2"></b-col>
              <b-col sm="4" class="text-start">
                <label :for="`type-${tag.label}`" class="p-2">{{tag.label}}</label> 
                <input v-model="tag.value" :type="tag.inputType" :required="tag.required" :value="tag.default" @change="emitEvent(tag)" >
                <small v-if="tag.help" class="text-muted">{{tag.help}}</small>
              </b-col>
            </b-row>

            <b-row class="my-4" v-if="tag.type === 'choice'">
              <b-col sm="2" class="text-end">
                <label :for="`type-${tag.label}`">{{tag.label}}:</label>
              </b-col>
              <b-col sm="4" class="text-start">
                <select v-model="tag.value" :required="tag.required" as="select" class="form-control" @change="emitEvent(tag)">
                  <option value="">Please Choose...</option>
                  <option :value="opt.value" v-for="opt in tag.options" :key="opt.value">{{opt.name}}</option>
                </select>
                <small v-if="tag.help" class="text-muted">{{tag.help}}</small>
              </b-col>
            </b-row>

          </b-col>
        </b-row>
    </b-container>
  </div> 
</template>

<script>
import { validate } from 'vee-validate';

export default {
  name: 'tagTemplate',
  props: {
    tags:{
      type: Array,
      require: true
    },
  },
  data(){
    return{
      values:[]
    }
  },
  methods: {
    async emitEvent(tag){
        
        let isValid = true;
        let data = tag.prefix +":"+tag.value;

        //https://vee-validate.logaretm.com/v3/guide/rules.html#rules
        if(tag.inputType === 'text' && tag.required){
         isValid = await validate(tag.value, 'required|min:3|alpha').then(result => {
            return result.valid
          });
        }

        if(tag.inputType === 'number' && tag.required){
         isValid = await validate(tag.value, 'required|numeric').then(result => {
            return result.valid
          });
        }

        if(tag.inputType === 'checkbox' && tag.required){
         isValid = await validate(tag.value, 'required').then(result => {
            return result.valid
          });
        }

        if(tag.inputType === 'select' && tag.required){
         isValid = await validate(tag.value, 'required').then(result => {
            return result.valid
          });
        }
        
        if(isValid){
          //console.log("tagString==> emit ", data);
          this.$emit('tag-string', data)
        }
        
        
      }
  }
}
</script>

<style scoped>
</style>
