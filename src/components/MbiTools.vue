<template>

  <div class="container">

  <div class="p-3">
    <h6>MBI Generator</h6>
    <button @click="generateMBI" class="btn btn-primary">Generate MBI</button>
    <span id="generated-mbi" class="m-2 text-muted">{{ mbi }}</span>
    <img v-if="mbi" @click="copyMbiToClipboard" src="../assets/copy.png" alt="Copy to clipboard" class="cursor-pointer" width="20" height="20" />
  </div>

  <div class="p-3">
    <div class="form-group">
      <label for="mbi-test">MBI Verifier</label>
      <input :class="mbiVerifyResult && mbiVerifyResult.class" class="form-control" v-on:input="mbiVerifyResult = null"
             type="text" id="mbi-test" name="mbi-test" v-model="mbiTest" placeholder="Enter MBI"/>
    </div>
    <button @click="verifyMBI" :disabled='!mbiTest' class="btn btn-primary">Verify MBI</button>
    <span v-if="mbiVerifyResult" id="validation-result" class="m-2 text-muted">
      {{ mbiVerifyResult.valid ? "This is a valid MBI" : "This is an invalid MBI" }}
    </span>
  </div>

  </div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'MbiTools',
  devServer: {
    // This will forward any request that does not match a static file to localhost:3000
    proxy: 'http://localhost:8080'
  },
  data() {
    return {
      mbi: '',
      mbiTest: '',
      mbiVerifyResultClass: '',
      mbiVerifyResult: null
    }
  },
  methods: {
    generateMBI: function () {
      axios.get(process.env.VUE_APP_MBI_API_URL + `/generate`)
          .then(response => {
            // JSON responses are automatically parsed.
            this.mbi = response.data.result
          })
          .catch(e => {
            console.error(e);
            throw new Error('Server-side error generating MBI')
          })
    },

    copyMbiToClipboard: function() {
      navigator.clipboard.writeText(this.mbi).then(() => {
        alert("Copied to clipboard");
      });
    },

    verifyMBI: function () {
      axios.post(process.env.VUE_APP_MBI_API_URL + `/verify`, {mbi: this.mbiTest})
          .then(response => {
            // JSON responses are automatically parsed.
            console.log(response.data.valid);
            this.mbiVerifyResult = response.data;
            this.mbiVerifyResult.class = this.mbiVerifyResult.valid ? 'is-valid' : 'is-invalid';
            console.log(this.mbiVerifyResult);
          })
          .catch(e => {
            console.error(e);
            throw new Error('Server-side error verifying MBI')
          })
    }
  }

}
</script>

<style>
.cursor-pointer{
  cursor: pointer;
}
</style>

