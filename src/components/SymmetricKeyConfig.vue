<template lang="html">
  <div>
		<input class="input" placeholder="Key generation password (type whatever you want...)" v-model="symPassword" @input="updateKey" /><br>
		Symmetric key ID: {{symKeyId || "Type a password to start generating the key"}}
	</div>
</template>

<script>
export default {
  props: ["symKeyId"],
  data() {
    return {
      symPassword: ""
    }
  },
  methods: {
    updateKey() {
      this.$emit("update-sym-key", this.symPassword);
    }
  }
}
</script>

<style lang="css">
</style>
