<!--<template>-->
<!--  <div id="container" class="editor" v-bind:style="{width: width, height: height}"></div>-->
<!--</template>-->

<!--<script lang="ts">-->
<!--import {Component, Prop, Vue} from "nuxt-property-decorator"-->
<!--import * as monaco from 'monaco-editor'-->
<!--import IStandaloneCodeEditor = monaco.editor.IStandaloneCodeEditor;-->


<!--export default class extends Vue {-->
<!--  @Prop() value: string | undefined-->
<!--  @Prop() width: string| undefined-->
<!--  @Prop() height: string| undefined-->
<!--  editor: IStandaloneCodeEditor | undefined-->

<!--  mounted() {-->
<!--    const root = document.getElementById('container')-->
<!--    if (root !== null) {-->
<!--      this.editor = monaco.editor.create(root, {-->
<!--        value: this.value,-->
<!--      });-->
<!--      if (this.value != null) {-->
<!--        const model = monaco.editor.createModel(this.value)-->
<!--        this.editor.setModel(model)-->
<!--      }-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->
