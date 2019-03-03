<template lang="html">
  <div class="header">
    <h1 class="title">Decentralized Chat App</h1>
    <p class="subtitle">
      Made with &hearts; and &#x2615; by <a href="http://www.sontbv.xyz/" target="_blank">sontbv</a>
    </p>
    <b-taglist class="is-flex taglist">
      <b-tag rounded>Whisper</b-tag>
      <b-tag rounded>Swarm</b-tag>
      <b-tag rounded>Vuejs</b-tag>
      <b-tag rounded>Bulma</b-tag>
    </b-taglist>
  </div>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style lang="scss" scoped>
.header {
  margin-top: 50px;
  margin-bottom: 50px;
  .title {
    margin-bottom: 30px;
  }
}

.taglist {
  justify-content: center;
}

</style>
