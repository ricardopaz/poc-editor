<template>
  <div id="text-editor">
    <h4>Editor com Autocomplete</h4>
    <p>
      Para usar o autocomplete das tags basta inserir a chave "<b>{</b>" ou usar o menu "<b>Tags</b>".
    </p>

    <div id="toolbar">
      <span class="ql-formats">
        <button class="ql-bold"></button>
        <button class="ql-italic"></button>
        <button class="ql-underline"></button>
        <button class="ql-strike"></button>
        <button class="ql-blockquote"></button>
      </span>

      <span class="ql-formats">
        <select class="ql-placeholder">
          <option 
            :key="placeholder.id" 
            :value="placeholder.id"
            v-for="placeholder in placeholders" 
          >
            {{placeholder.label}}
          </option>
        </select>
      </span>
    </div>

    <div id="editor"></div>
    <div id="completions"></div>
  </div>
</template>

<script>
import '@vueup/vue-quill/dist/vue-quill.snow.css'

import { Quill } from '@vueup/vue-quill'

import getPlaceholderModule from 'quill-placeholder-module'
import getAutocompleteModule from 'quill-placeholder-autocomplete-module'

Quill.register('modules/placeholder', getPlaceholderModule(Quill))
Quill.register('modules/autocomplete', getAutocompleteModule(Quill))

export default {
  name: 'TextEditor',

  data() {
    return {
      placeholders: [
        {id: 'locatarioNome', label: 'Nome do Locatário'},
        {id: 'locatarioCpf', label: 'CPF  do Locatário'},
        {id: 'locatarioRG', label: 'RG  do Locatário'},
        {id: 'locatarioEndereco', label: 'Endereço do Locatário'},
        {id: 'locatarioRenda', label: 'Renda do Locatário'},
        {id: 'required', label: 'Informação Obrigatória', required: true}
      ]
    }
  },

  mounted() {
    const quillProps = {
      modules: {
        toolbar: { container: '#toolbar' },
        placeholder: {
          delimiters: ['{', '}'],
          placeholders: this.placeholders,
        },
        autocomplete: {
          endKey: '}',                              // optional
          triggerKey: '{',                          // default
          debounceTime: 0,                          // 0: disabled (default)
          container: '#completions',                // can also be return of `document.querySelector` or kept `undefined`
          getPlaceholders: () => this.placeholders,
          // onOpen: () => console.log('opened'),                                      // optional
          // onClose: (placeholder) => console.log('user choosed:', placeholder),      // optional
          // fetchPlaceholders: (query) => fetch(...).then(...),                     // optional
          // onFetchStarted: (query) => console.log('user searching for:', query),     // optional
          // onFetchFinished: (results) => console.log('possible results:', results),  // optional
        },
      },
      placeholder: 'Compose an epic...',
      theme: 'snow'
    }

    new Quill('#editor', quillProps)
  }
}
</script>

<style>
#text-editor {
  width: 50%;
  height: 300px;
}

h4 {
  margin: 0 0 10px 0;
}

.suggest {
  color: blue;
}

.ql-placeholder-content {
  color: green;
}

.ql-picker.ql-placeholder {
  width: 118px;
}

.ql-picker.ql-placeholder > span.ql-picker-label::before {
  content: 'Tags';
}

.ql-picker.ql-placeholder > span.ql-picker-options > span.ql-picker-item::before {
  content: attr(data-label);
}
</style>
