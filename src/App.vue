<template>
  <div class="editor">
    <editor-menu-bar :editor="editor">
      <div class="menubar">
        <button
          class="menubar__button"
          @click="showVariablesModal()"
        >
          Add Variables
        </button>
      </div>
    </editor-menu-bar>
    <editor-content class="editor__content" :editor="editor" />
    <div v-if="modalState" id="open-modal" class="modal-window">
      <div>
        <a @click="closeVariablesModal()" title="Close" class="modal-close">Close</a>
        <input type="radio" id="variablea" v-model="variable" value="okay">
        <label for="variablea">Variable A</label>
        <input type="radio" id="variableb" v-model="variable" value="google">
        <label for="variableb">Variable B</label>
      </div>
    </div>
  </div>
</template>

<script>
  import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
  import './index.scss';
  export default {
    components: {
      EditorMenuBar,
      EditorContent
    },
    data() {
      return {
        modalState: false,
        variable: '',
        editor: new Editor({
          content: `
          <h2>
            Embeds
          </h2>
          <p>
            This is an example of a custom iframe node. This iframe is rendered as a <strong>vue component</strong>. This makes it possible to render the input below to change its source.
          </p>
        `,
        }),
      }
    },
    methods: {
      closeVariablesModal() {
        const position = this.editor.state.selection.anchor;
        const variable = this.variable;
        const transaction = this.editor.state.tr.insertText(variable, position);
        this.editor.view.dispatch(transaction);
        this.modalState = !this.modalState;
      },
      showVariablesModal() {
        this.modalState = !this.modalState;
      },
    },
    beforeDestroy() {
      this.editor.destroy()
    },
  }
</script>

