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

        <b-alert
          :show="dismiss_countdown"
          dismissible
          fade
          variant="success"
          @dismiss-count-down="countdown_changed"
        >
          Content copied to clipboard
        </b-alert>
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
export default {
  name: "Formatter",

  data() {
    return {
      to_convert: null,
      converted: null,
      dismiss_secs: 3,
      dismiss_countdown: 0,
    }
  },

  methods: {
    convert() {
      let content = this.to_convert

      // Converte aspas simples em aspas duplas
      content = content.replaceAll("'", "\"")
      // Altera valores "None" (python) em "null"
      content = content.replaceAll("None", null)

      try {
        const json_object = JSON.parse(content)
        this.converted = JSON.stringify(json_object, null, 4)
      } catch (error) {
        this.converted = "Invalid JSON"
      }
    },

    onCopy: function () {
      this.show_alert()
    },

    onError: function (e) {
      alert('Failed to copy the text to the clipboard')
      console.log(e)
    },

    countdown_changed(dismiss_countdown) {
      this.dismiss_countdown = dismiss_countdown
    },

    show_alert() {
      this.dismiss_countdown = this.dismiss_secs
    },

  }
}
</script>
