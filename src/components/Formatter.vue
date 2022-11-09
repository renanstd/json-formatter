<template>
  <b-container fluid>
    <b-row>

      <b-col cols="6">
        <span>Paste your ugly string here</span>
        <b-form-textarea
          v-model="to_convert"
          rows="30"
          @input="convert"
        ></b-form-textarea>
      </b-col>

      <b-col cols="6">
        <span>Here's your beautiful json</span>
        <b-form-textarea
          id="result"
          v-model="converted"
          rows="30"
          wrap="off"
          plaintext
        ></b-form-textarea>

        <b-button
          block
          v-clipboard:copy="converted"
          v-clipboard:success="onCopy"
          v-clipboard:error="onError"
        >
          Copy
        </b-button>
      </b-col>

    </b-row>

    <br/>
    <br/>

    <b-row>
      <b-col>
        <Footer/>
      </b-col>
    </b-row>
  </b-container>
</template>

<style scoped>
#result {
  font-family: monospace;
}
</style>

<script>
import Footer from '@/components/Footer.vue'

export default {
  name: "Formatter",
  components: {Footer},

  data() {
    return {
      to_convert: null,
      converted: null,
    }
  },

  methods: {
    convert() {
      let content = this.to_convert

      // Converte aspas simples em aspas duplas
      content = content.replaceAll("'", "\"")
      // Altera valores "None" (python) em "null"
      content = content.replaceAll("None", null)
      // Altera valores "True" (python) para "true"
      content = content.replaceAll("True", true)
      // Altera valores "False" (python) para "false"
      content = content.replaceAll("False", false)

      try {
        const json_object = JSON.parse(content)
        this.converted = JSON.stringify(json_object, null, 4)
      } catch (error) {
        this.converted = "Invalid JSON"
      }
    },

    onCopy: function () {
      this.$bvToast.toast(
        "Content copied to clipboard",
        {autoHideDelay: 5000, title: "Success"}
      )
    },

    onError: function (e) {
      alert('Failed to copy the text to the clipboard')
      console.log(e)
    },

  }
}
</script>
