<template>
<div>
      <h1>Список пользователей</h1>
    <div class="input-section">
      <input type="text" 
             placeholder="Поиск по имени или e-mail"
             v-model="txtInput"
             @keyup="emitToParent"
             >
      <div class="clear-section"
           v-show="txtInput.length > 0 || isSorted == true"
           @click="txtInput = ''; clearInputField(); sortReset();"
           >
        <div class="clear-filter">Очистить фильтр</div>
      </div>
    </div>
    </div>
</template>

<script>
export default {
  props: {
    isSorted: Boolean,
    sortReset: Function,
  },

  emits: ['childToParent', 'clearField'],
   data() {
     return {
       txtInput: "",
     }
   },

  methods: {
    emitToParent() {
      this.$emit('childToParent', this.txtInput)
    }, 
    clearInputField () {
      this.$emit('clearField', )
    }
  }
}
</script>
