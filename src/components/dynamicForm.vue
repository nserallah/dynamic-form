<template>
  <div class="my-form">
      <h2>My Dynamic form</h2>
      <form>
        <div v-for="(field, index) in formData" :key="index" class="form-group">
          <label :for="field.name">{{field.label}}</label>
          <input
          :class="field.type !== 'checkbox' ? 'form-control' : ''"
          v-if="field.type === 'text' || field.type === 'password' || field.type === 'checkbox'" 
          :id="field.name" 
          :type="field.type" 
          :name="field.name"
          @change="handleValue($event)"
          >
          <textarea 
          class="form-control"
          v-if="field.type === 'textarea'"
          :name="field.name"
          @change="handleValue($event)"
          ></textarea>
          <select
            class="form-control"
            :name="field.name"
            v-if="field.type === 'option'"
            @change="handleValue($event)">
            <option v-for="(option, index) in field.options" :key="index">
              {{ option }}
            </option>
          </select>
        
        </div>
        <p v-if="errorMessage">Youser name and password is required and and min length 6</p>
        <input class="btn btn-success" @click.prevent="handleSubmit" type="submit" value="Save">
       </form>
  </div>
</template>

<script>
import formjson from "../json/form.json"
export default {
    data() {
      return {
        formData: {},
        errorMessage: false
      }
    },
    methods: {
      handleValue(event) {
        this.formData[event.target.name].value = event.target.value
        
      },
      formHasError() {
        let error;
        Object.keys(this.formData).forEach(key => {
          
          if(this.formData[key].min === 6){
            if(!this.formData[key].value || this.formData[key].value.length < this.formData[key].min) {
              error = true
            } else {
              error = false
            }
          }
        })
        return error
      },
      handleSubmit() {
        this.formHasError() ? this.errorMessage = true : this.errorMessage = false
      }
    },
    mounted() {
      this.formData = formjson;
    }
    
}
</script>

<style>
.my-form {width: 300px; margin: auto}

</style>