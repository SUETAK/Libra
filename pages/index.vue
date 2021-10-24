<template>
  <div>
    <v-container>
      <v-row>
        <v-col>
          <v-text-field label="requestURL"/>
          <div class="d-flex justify-space-around pa-6">
            <v-flex>
              <v-expansion-panels value="expanded" class="px-2">
                <v-expansion-panel>
                  <v-expansion-panel-header class="d-flex justify-space-around">common-header</v-expansion-panel-header>
                  <v-expansion-panel-content v-for="header in commonHeader" v-bind="header">
                    <div class="d-flex justify-space-around">
                      <v-text-field label="commonHeader-key" class="px-6"/>
                      <v-text-field label="commonHeader-value" class="px-6"/>
                    </div>
                  </v-expansion-panel-content>
                </v-expansion-panel>
              </v-expansion-panels>
            </v-flex>
            <v-btn class="pa-6" @click="addHeader(commonHeader)">addHeader</v-btn>
          </div>
          <v-row class="d-flex justify-space-around pa-6">
            <div class="d-flex justify-space-around pa-6">
              <v-flex>
                <v-expansion-panels class="px-2">
                  <v-expansion-panel>
                    <v-expansion-panel-header class="d-flex justify-space-around">left-header</v-expansion-panel-header>
                    <v-expansion-panel-content v-for="header in leftHeader" v-bind="header">
                      <div class="d-flex justify-space-around">
                        <v-text-field label="leftHeader-key" class="px-6"/>
                        <v-text-field label="leftHeader-value" class="px-6"/>
                      </div>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-flex>
              <v-btn class="pa-6" @click="addHeader(leftHeader)">addHeader</v-btn>
            </div>
            <div class="d-flex justify-space-around pa-6">
              <v-flex>
                <v-expansion-panels class="px-2">
                  <v-expansion-panel>
                    <v-expansion-panel-header class="d-flex justify-space-around">right-header</v-expansion-panel-header>
                    <v-expansion-panel-content v-for="header in rightHeader" v-bind="header">
                      <div class="d-flex justify-space-around">
                        <v-text-field label="rightHeader-key" class="px-6"/>
                        <v-text-field label="rightHeader-value" class="px-6"/>
                      </div>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-flex>
              <v-btn class="pa-6" @click="addHeader(rightHeader)">addHeader</v-btn>
            </div>
          </v-row>
          <v-btn class="d-flex justify-center mb-6">submit</v-btn>
        </v-col>
      </v-row>
    </v-container>

    <MonacoEditor
      :diffEditor="true"
      :original="left"
      :value="right"
      class="editor"
      language="javascript"
      ref="diffViewEditor"
      :options="options"
    />
  </div>
</template>

<script>
import MonacoEditor from 'vue-monaco'

export default {
  components: {
    MonacoEditor
  },
  data() {
    return {
      commonHeader: [{
        key: "",
        value: ""
      }],
      leftHeader: [{
        key: "",
        value: ""
      }],
      rightHeader: [{
        key: "",
        value: ""
      }],
      options: {
        enableSplitViewResizing: false,
        // Render the diff inline
        renderSideBySide: true,
        LineNumbersType: "on",
        BuiltinTheme: "vs-dark"
      },
      left: `import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
`,
      right: `module.exports = {
  root: true,
  env: {
    node: true
  },
  'extends': [
    'plugin:vue/essential',
    'eslint:recommended'
  ],
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off'
  },
  parserOptions: {
    parser: 'babel-eslint'
  }
}
`
    }
  },
  // send を押す
  // リクエストが行われる
  // await でレスポンスが返却されるまで待機
  // リクエストが返却される
  // レスポンスで値を変更
  // right, left の値の変化をwatch, computed で監視

  methods: {

    request(url, header) {

    },
    addHeader(header) {
      header.push({
        key: "",
        value: ""
      })
    },
    setLang(lang) {
      monaco.editor.setModelLanguage(
        this.$refs.diffViewEditor.getModifiedEditor().getModel(),
        lang
      )
      monaco.editor.setModelLanguage(
        this.$refs.diffViewEditor.getEditor().getModel(),
        lang
      )
    },
    setTheme(theme) {
      monaco.editor.setTheme(theme)
    }
  }
}
</script>

<style>
.editor {
  width: 100%;
  height: 800px;
}
</style>
