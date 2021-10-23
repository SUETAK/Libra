<template>
  <div id="container" class="editor" v-bind:style="{width: width, height: height}"></div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from "nuxt-property-decorator"
import * as monaco from 'monaco-editor';
import IStandaloneCodeEditor = monaco.editor.IStandaloneCodeEditor;

@Component({
  watch: {
    value(newValue) {
      this.editor.getModel().setValue(newValue)
    }
  }
})
export default class extends Vue {
  @Prop() value: string
  @Prop() width: string
  @Prop() height: string
  editor: IStandaloneCodeEditor

  mounted() {
    const root = document.getElementById('container')
    if (root !== null) {
      this.editor = monaco.editor.create(root, {
        value: this.value,
      });
      const model = monaco.editor.createModel(this.value)
      this.editor.setModel(model)
    }
  }
}
</script>
