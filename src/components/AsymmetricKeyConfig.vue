<template lang="html">
  <div>
		Asymetric key id: {{keyId}}
    <br>
		Public key: {{pubKey}}
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
      console.log("son123")
      this.$emit("new-keypair");
    }
  }
}
</script>

<style lang="css">
</style>
