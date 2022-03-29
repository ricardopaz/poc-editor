<template>
  <QuillEditor />
  <!-- <TinyEditor /> -->
</template>

<script>
// import TinyEditor from './components/Tiny.vue'
import QuillEditor from './components/Quill.vue'

export default {
  name: 'App',
  components: {
    QuillEditor,
    // TinyEditor
  }
}
</script>

<style>
html,
body,
#app {
  padding: 0;
  margin: 0;
  height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
