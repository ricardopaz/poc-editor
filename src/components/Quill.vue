<template>
  <div id="text-editor">
    <div id="toolbar">
      <span class="ql-formats">
        <select class="ql-size">
          <option value="small"></option>
          <option selected></option>
          <option value="large"></option>
          <option value="huge"></option>
        </select>
        <button class="ql-bold"></button>
        <button class="ql-italic"></button>
        <button class="ql-underline"></button>
        <select class="ql-align">
          <option selected></option>
          <option value="center"></option>
          <option value="right"></option>
          <option value="justify"></option>
        </select>
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

    <div id="editor">
      <h2 class="ql-align-center">CONTRATO DE LOCAÇÃO DE IMÓVEL</h2>
      <p class="ql-align-center"><br></p>
      <p><strong>LOCADOR</strong>:</p>
      <p><span class="ql-placeholder-content" data-id="locadorNome" data-label="Nome do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{Nome do Locador}</span>&#xFEFF;</span>, de nacionalidade <span class="ql-placeholder-content" data-id="locadorNacionalidade" data-label="Nacionalidade do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{Nacionalidade do Locador}</span>&#xFEFF;</span>, <span class="ql-placeholder-content" data-id="locadorVinculoEmpregaticio" data-label="Vinculo Empregatício do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{Vinculo Empregatício do Locador}</span>&#xFEFF;</span>, casada, RG. <span class="ql-placeholder-content" data-id="locadorRG" data-label="RG  do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{RG  do Locador}</span>&#xFEFF;</span> e CPF <span class="ql-placeholder-content" data-id="locadorCpf" data-label="CPF  do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{CPF  do Locador}</span>&#xFEFF;</span>, residente e domiciliada, nesta data, na <span class="ql-placeholder-content" data-id="locadorEndereco" data-label="Endereço do Locador" spellcheck="false">&#xFEFF;<span contenteditable="false">{Endereço do Locador}</span>&#xFEFF;</span>.</p>
      <p><br></p><p><strong>LOCATÁRIO</strong>:</p>
      <p><span class="ql-placeholder-content" data-id="locatarioNome" data-label="Nome do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{Nome do Locatário}</span>&#xFEFF;</span>, de nacionalidade <span class="ql-placeholder-content" data-id="locatarioNacionalidade" data-label="Nacionalidade do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{Nacionalidade do Locatário}</span>&#xFEFF;</span>, solteira, <span class="ql-placeholder-content" data-id="locatarioVinculoEmpregaticio" data-label="Vinculo Empregatício do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{Vinculo Empregatício do Locatário}</span>&#xFEFF;</span>, RG. <span class="ql-placeholder-content" data-id="locatarioRG" data-label="RG  do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{RG  do Locatário}</span>&#xFEFF;</span> e CPF <span class="ql-placeholder-content" data-id="locatarioCpf" data-label="CPF  do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{CPF  do Locatário}</span>&#xFEFF;</span>, nesta data residente e domiciliada na <span class="ql-placeholder-content" data-id="locatarioEndereco" data-label="Endereço do Locatário" spellcheck="false">&#xFEFF;<span contenteditable="false">{Endereço do Locatário}</span>&#xFEFF;</span>.</p>
    </div>

    <div id="completions" />
  </div>
</template>

<script>
import '@vueup/vue-quill/dist/vue-quill.snow.css'

import { Quill } from '@vueup/vue-quill'

import QuillBetterTable from "quill-better-table";
import getPlaceholderModule from 'quill-placeholder-module'
import getAutocompleteModule from 'quill-placeholder-autocomplete-module'


Quill.register("modules/better-table", QuillBetterTable);
Quill.register('modules/placeholder', getPlaceholderModule(Quill))
Quill.register('modules/autocomplete', getAutocompleteModule(Quill))

export default {
  name: 'QuillEditor',

  data() {
    return {
      placeholders: [
        {id: 'locatarioNome', label: 'Nome do Locatário'},
        {id: 'locatarioNacionalidade', label: 'Nacionalidade do Locatário'},
        {id: 'locatarioVinculoEmpregaticio', label: 'Vinculo Empregatício do Locatário'},
        {id: 'locatarioCpf', label: 'CPF  do Locatário'},
        {id: 'locatarioRG', label: 'RG  do Locatário'},
        {id: 'locatarioEndereco', label: 'Endereço do Locatário'},
        {id: 'locatarioRenda', label: 'Renda do Locatário'},
        
        {id: 'locadorNome', label: 'Nome do Locador'},
        {id: 'locadorNacionalidade', label: 'Nacionalidade do Locador'},
        {id: 'locadorVinculoEmpregaticio', label: 'Vinculo Empregatício do Locador'},
        {id: 'locadorCpf', label: 'CPF  do Locador'},
        {id: 'locadorRG', label: 'RG  do Locador'},
        {id: 'locadorEndereco', label: 'Endereço do Locador'},
        {id: 'locadorRenda', label: 'Renda do Locador'},
      ]
    }
  },

  mounted() {
    const appendTitleOnCompletions = () => {
      var completions = document.getElementById('completions')
      var titleElement = document.createElement('div');
      titleElement.innerHTML = `
        <h2>Variáveis</h2>
        <p>Você pode navegar pelas setas do teclado e selecionar as tags com enter.</p>
        <h3>Tags</h3>
      `;

      completions.insertBefore(titleElement, completions.firstChild);
    }

    const quillProps = {
      theme: 'snow',
      placeholder: 'Copie e cole seu contrato aqui',
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
          onOpen: appendTitleOnCompletions,                                      // optional
          getPlaceholders: () => this.placeholders,
          // onClose: (placeholder) => console.log('user choosed:', placeholder),      // optional
          // fetchPlaceholders: (query) => fetch(...).then(...),                     // optional
          onFetchStarted: (query) => console.log('user searching for:', query),     // optional
          onFetchFinished: (results) => console.log('possible results:', results),  // optional
        },
      },
    }

    new Quill('#editor', quillProps)
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito+Sans:ital,wght@0,400;0,600;0,700;1,400;1,600;1,700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

#text-editor {
  width: 100%;
  height: 100%;
  overflow: scroll;
  background: #E5E5E5;
  font-family: Nunito Sans;
  display: grid;
  grid-template-areas:
    'toolbar toolbar'
    'editor completions';
  grid-template-rows: auto 1fr;
  grid-template-columns: 1fr auto;
}

#toolbar { 
  z-index: 9;
  grid-area: toolbar;
  position: sticky;
  border: none;
  padding: 15px 10%;
}

#editor { 
  grid-area: editor;
}

#completions { 
  width: 250px;
  padding: 30px;
  color: #50575A;
  top: 0 !important;
  left: 0 !important;
  grid-area: completions;
  position: relative !important;
  border-left: 1px solid #D6D6D6;
}

#completions h2 {
  font-size: 20px;
  font-style: normal;
  font-weight: 700;
  line-height: 32px;
  letter-spacing: 0px;
  text-align: left;
}

#completions p {
  font-size: 16px;
  font-style: normal;
  font-weight: 400;
  line-height: 24px;
  letter-spacing: 0px;
}

#completions h3 {
  font-size: 18px;
  font-style: normal;
  font-weight: 600;
  line-height: 32px;
  letter-spacing: 0px;
  text-align: left;
}

#completions li {
  appearance: none;
  display: block;
}

#completions li button {
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  appearance: none;
  border-radius: 4px;
  background: #FFFFFF;
  box-sizing: border-box;
  border: 1px solid #D6D6D6;
  font-size: 12px;
  line-height: 18px;
  color: #474747;
  outline-color: #009885;
}

.ql-editor {
  line-height: 28px;
  font-family: Montserrat;
}

.ql-container {
  height: 100%;
  width: 840px;
  margin: 20px auto;
  padding: 60px 115px;
  background: white;
}

.suggest,
.ql-placeholder-content {
  background: #E0F4F2;
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
