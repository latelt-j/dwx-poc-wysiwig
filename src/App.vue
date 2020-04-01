<template>
  <div class="editor">
    <h1 class="title">POC WYSIWIG TEMPLATE MAIL</h1>
    <editor-menu-bar :editor="editor" v-slot="{ commands, isActive }">
      <div class="menubar">
        <button
          class="button-bar"
          @click="showVariablesModal()"
        >
          Add Variables
        </button>
        <button class="button-bar" :class="{ 'is-active': isActive.bold() }" @click="commands.bold">
          Bold
        </button>
        <button class="button-bar" :class="{ 'is-active': isActive.heading({ level: 2 }) }" @click="commands.heading({ level: 2 })">
          H2
        </button>
      </div>
    </editor-menu-bar>
    <editor-content class="editor-content" :editor="editor" />
    <div v-if="modalState" id="open-modal" class="modal-window">
      <div>
        <a @click="closeVariablesModal()" title="Close" class="modal-close">Close</a>
        <input type="radio" id="variablea" v-model="variable" value="okay">
        <label for="variablea">Variable A</label>
        <input type="radio" id="variableb" v-model="variable" value="google">
        <label for="variableb">Variable B</label>
      </div>
    </div>
    <div class="footer">
      Made with <img class="picto-vuejs" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Vue.js_Logo_2.svg/1200px-Vue.js_Logo_2.svg.png">
    </div>
  </div>
</template>

<script>
  import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
  import {
    Heading,
    Bold
  } from 'tiptap-extensions'
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
          extensions: [
            new Heading(),
            new Bold(),
          ],
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

