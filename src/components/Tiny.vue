<template>
  <div id="text-editor">
    <h4>Editor com Autocomplete</h4>

    <editor
      apiKey="kz05zriy0haut8nlh09sd0c2631s4eca1qrx4992gd8moa9c"
      :init="{
        height: 500,
        menubar: false,

        plugins: [
          'mentions',
          'advlist autolink lists link image charmap print preview anchor',
          'searchreplace visualblocks code fullscreen',
          'insertdatetime media table paste code help wordcount'
        ],

        toolbar:
          'undo redo | fontsizeselect | bold italic underline | \
          bullist numlist outdent indent | \
          alignleft aligncenter alignright alignjustify | removeformat | help',

        content_style: '.mymention{ color: gray; }' +
          'body { font-family:Helvetica,Arial,sans-serif; font-size:14px }',

        mentions_item_type: 'name',
        mentions_fetch: mentions_fetch,
        mentions_selector: '.mymention',
        mentions_select: mentions_select,
        mentions_menu_hover: mentions_menu_hover,
        mentions_menu_complete: mentions_menu_complete,
      }"
     />
  </div>
</template>

<script>
import Editor from '@tinymce/tinymce-vue'

const placeholders = [
  { id: "1", name: 'Tag 1' },
  { id: "2", name: 'Tag 2' },
  { id: "3", name: 'Tag 3' },
  { id: "4", name: 'Tag 4' },
  { id: "5", name: 'Tag 5' },
]

export default {
  name: 'TinyEditor',

  components: {
    'editor': Editor
  },

  methods: {
    fetchPlaceholder(id) {
      return placeholders.find(tag => tag.id === id)
    },

    mentions_fetch(query, success) {
      /* `query.term` is the text the user typed after the '@' */
      let placeholdersFiltered = placeholders.filter(user => {
        return user.name.toLowerCase().indexOf(query.term.toLowerCase()) !== -1;
      })

      placeholdersFiltered = placeholdersFiltered.slice(0, 10)

      /* Where the user object must contain the properties `id` and `name`
        but you could additionally include anything else you deem useful. */
      success(placeholdersFiltered)
    },

    mentions_menu_hover(replaceData, success) {
      const placeholderData = this.fetchPlaceholder(replaceData.id)
      var div = document.createElement('div');

      div.innerHTML = (
        '<div class="card">' +
          '<h1>' + placeholderData.name + '</h1>' +
        '</div>'
      );

      success(div);
    },

    mentions_menu_complete(editor, replaceData) {
      var span = editor.getDoc().createElement('span');
      span.className = 'mymention';
      span.setAttribute('data-mention-id', replaceData.id);
      span.appendChild(editor.getDoc().createTextNode('@' + replaceData.name));

      return span;
    },

    mentions_select(mention, success) {
      /* `mention` is the element we previously created with `mentions_menu_complete`
        in this case we have chosen to store the id as an attribute */
      const id = mention.getAttribute('data-mention-id');
    
      const div = document.createElement('div');
      const placeholderData = this.fetchPlaceholder(id);
      
      div.innerHTML = (
        '<div class="card">' +
          '<h1>' + placeholderData.name + '</h1>' +
        '</div>'
      );

      success(div);
    }
  },
}
</script>

<style>
div.card,
.tox div.card {
  width: 240px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 3px;
  box-shadow: 0 4px 8px 0 rgba(34, 47, 62, .1);
  padding: 8px;
  font-size: 14px;
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Oxygen-Sans,Ubuntu,Cantarell,"Helvetica Neue",sans-serif;
}

div.card::after,
.tox div.card::after {
  content: "";
  clear: both;
  display: table;
}

div.card h1,
.tox div.card h1 {
  font-size: 14px;
  font-weight: bold;
  margin: 0 0 8px;
  padding: 0;
  line-height: normal;
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Oxygen-Sans,Ubuntu,Cantarell,"Helvetica Neue",sans-serif;
}

div.card p,
.tox div.card p {
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Oxygen-Sans,Ubuntu,Cantarell,"Helvetica Neue",sans-serif;
}

div.card img.avatar,
.tox div.card img.avatar {
  width: 48px;
  height: 48px;
  margin-right: 8px;
  float: left;
}
</style>
