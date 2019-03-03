<template lang="html">
  <div>
    <b-tooltip label="Your asymetric key id"
      position="is-left">
      <button class="button is-dark">
        {{keyId}}
      </button>
    </b-tooltip>
    
    <br>
    
    <b-tooltip label="Your asymetric public key"
      position="is-left">
      <button class="button is-dark">
        {{pubKey}}
      </button>
    </b-tooltip>
    
    <div class="container">
      <p>
        <b-input v-model="pubKey" disabled></b-input>
      </p>
      
    </div>
	</div>
</template>

<script>
export default {
  props: ["pubKey", "keyId"],
  data() {
    return {
      /*keyId: this.prop.keyId,
      pubKey: this.prop.pubKey*/
    };
  },
  methods: {
    generateKey() {
      /* eslint-disable no-console */
      this.$emit("new-keypair");
    }
  }
}
</script>

<style lang="scss">

</style>
