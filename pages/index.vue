<template>
  <div>
    <v-container>
      <v-row>
        <v-col>
          <v-text-field label="requestURL" v-model="requestUrl"/>
          <div class="d-flex justify-space-around pa-6">
            <v-flex>
              <v-expansion-panels value="expanded" class="px-2">
                <v-expansion-panel>
                  <v-expansion-panel-header class="d-flex justify-space-around">common-header</v-expansion-panel-header>
                  <v-expansion-panel-content v-for="header in commonHeader">
                    <div class="d-flex justify-space-around">
                      <v-text-field label="commonHeader-key" v-model="header.key" class="px-6"/>
                      <v-text-field label="commonHeader-value" v-model="header.value" class="px-6"/>
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
                    <v-expansion-panel-content v-for="header in leftHeader">
                      <div class="d-flex justify-space-around">
                        <v-text-field label="leftHeader-key" v-model="header.key" class="px-6"/>
                        <v-text-field label="leftHeader-value" v-model="header.value" class="px-6"/>
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
                    <v-expansion-panel-header class="d-flex justify-space-around">right-header
                    </v-expansion-panel-header>
                    <v-expansion-panel-content v-for="header in rightHeader">
                      <div class="d-flex justify-space-around">
                        <v-text-field label="rightHeader-key" v-model="header.key" class="px-6"/>
                        <v-text-field label="rightHeader-value" v-model="header.value" class="px-6"/>
                      </div>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-flex>
              <v-btn class="pa-6" @click="addHeader(rightHeader)">addHeader</v-btn>
            </div>
          </v-row>
          <v-btn class="d-flex justify-center mb-6" @click="submitRequest">submit</v-btn>
        </v-col>
      </v-row>
    </v-container>

    <MonacoEditor
      :diffEditor="true"
      :original="JSON.stringify(left, null, ' ')"
      :value="JSON.stringify(right , null, ' ')"
      class="editor"
      language="json"
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
      requestUrl: "",
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
      requestLeftHeader: [],
      requestRightHeader: [],
      options: {
        enableSplitViewResizing: false,
        // Render the diff inline
        renderSideBySide: true,
        LineNumbersType: "on",
        BuiltinTheme: "vs-dark"
      },
      left: "leftTest",
      right: "rightTest",
    }
  },
  // send を押す
  // リクエストが行われる
  // await でレスポンスが返却されるまで待機
  // リクエストが返却される
  // レスポンスで値を変更
  // right, left の値の変化をwatch, computed で監視
  mounted() {
    if (localStorage.getItem('requestUrl') != null) {
      this.requestUrl = localStorage.getItem('requestUrl')
    }
    if (localStorage.getItem('leftHeader') != null) {
      this.leftHeader = JSON.parse(localStorage.getItem('leftHeader'))
    }
    if (localStorage.getItem('rightHeader') != null) {
      this.rightHeader = JSON.parse(localStorage.getItem('rightHeader'))
    }
    if (localStorage.getItem('commonHeader') != null) {
      this.commonHeader = JSON.parse(localStorage.getItem('commonHeader'))
    }
  },
  methods: {
    saveLocalStorage() {
      // this.fullName = val + ' ' + this.lastName
      localStorage.setItem('requestUrl', this.requestUrl)
      localStorage.setItem('leftHeader', JSON.stringify(this.leftHeader))
      localStorage.setItem('rightHeader', JSON.stringify(this.rightHeader))
      localStorage.setItem('commonHeader', JSON.stringify(this.commonHeader))

    },
    headerMerge(requestHeader, addHeader) {
      return requestHeader.push(this.commonHeader, addHeader)
    },
    async submitRequest() {
      this.saveLocalStorage()
      await this.doLeftRequest(this.requestUrl, this.headerMerge(this.requestLeftHeader, this.leftHeader))
      await this.doRightRequest(this.requestUrl, this.headerMerge(this.requestRightHeader, this.rightHeader))
    },
    async doLeftRequest(url, header) {
      try {
        console.log("doLeftRequest")
        let self = this
        await this.$axios.get(url, {headers: JSON.stringify(header)}).then(function (response) {
          self.left = response.data
        }).catch(function (error) {
          console.log(error)
        })
      } catch (e) {
        console.log(e)
      }
    },
    async doRightRequest(url, header) {
      try {
        console.log("doRightRequest")
        let self = this
        await this.$axios.get(url, {headers: JSON.stringify(header)}).then(function (response) {
          self.right = response.data
        }).catch(function (error) {
          console.log(error)
        })
      } catch (e) {
        console.log(e)
      }
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
