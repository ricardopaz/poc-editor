<template>
  <TextEditor />
</template>

<script>
import TextEditor from './components/TextEditor.vue'

export default {
  name: 'App',
  components: {
    TextEditor
  }
}
</script>

<style>
#app {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
