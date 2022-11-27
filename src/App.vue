<template>
  <head>
    <title>Medicare Beneficiary Identifier Tools</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.3.1/dist/css/bootstrap.min.css"
          integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossOrigin="anonymous">
  </head>

  <html lang="en">
  <nav className="navbar navbar-light bg-light">
    <a className="navbar-brand" href="/">Medicare Beneficiary Identifier Tools</a>
  </nav>

  <MbiTools/>

  </html>
</template>

<script>
import MbiTools from './components/MbiTools.vue'

export default {
  name: 'App',
  components: {
    MbiTools
  }
}
</script>

