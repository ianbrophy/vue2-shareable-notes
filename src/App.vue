<template>
  <div class="container mt-4">
    <h3 class="mb-3">Shareable Notes</h3>
    <p> Type your notes in the textarea below.</p>
    <p>To share these notes, or bring them to a new browser, copy and paste the full url (incuding the data paramter and its value)</p>
    <textarea
      class="form-control"
      rows="10"
      v-model="notes"
      placeholder="Write your notes here..."
    ></textarea>
  </div>
</template>

<script>
import LZString from "lz-string"

export default {
  data() {
    return {
      notes: ""
    }
  },

  mounted() {
    this.loadFromURL()
  },

  watch: {
    notes() {
      this.updateURL()
    }
  },

  methods: {
    loadFromURL(){
      const data = new URLSearchParams(window.location.search).get("data")

      if (!data) return
      const decompressed = LZString.decompressFromEncodedURIComponent(data)
      this.notes = decompressed || ""

    },

    updateURL() {
      const compressed = LZString.compressToEncodedURIComponent(this.notes)
      const newURL = window.location.pathname + "?data=" + compressed

      window.history.replaceState({}, "", newURL)
    }
  }
}
</script>

