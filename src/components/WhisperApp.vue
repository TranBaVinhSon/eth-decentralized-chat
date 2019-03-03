<template lang="html">
  <div>
		<div v-if="!configured">
      <div class="is-flex ">
        <input class="input" placeholder="Type your name..." v-model="name" /><br>
      </div>
      <br>
      <div class="field">
        <b-checkbox v-model="asym">Asymmetric</b-checkbox>
      </div>
      
			<AsymmetricKeyConfig v-if="asym" :pub-key="asymPubKey" :key-id="asymKeyId"></AsymmetricKeyConfig>
			<SymmetricKeyConfig v-else @update-sym-key="updateSymKey" :sym-key-id="symKeyId"></SymmetricKeyConfig>
      
      <br>
      
      <button class="button-start" @click="configWithKey" v-if="(asymKeyId || symKeyId) && name">Start</button>
      
		</div>
		<div v-else>
			<div v-if="asym">
        <b-tooltip label="Your publick key"
          position="is-left">
          <button class="button is-dark">
            {{asymPubKey}}
          </button>
        </b-tooltip>
        <b-field label="Recipient public key">
          <b-input v-model="recipientPubKey"></b-input>
        </b-field>
			</div>
			<div class="is-flex" v-else>
				Key: {{symKeyId}}
			</div>
      <hr>
			<p class="is-flex chat-messages" v-for="m of msgs" :key="m.name">
				<b>{{m.name}}</b>: {{m.text}}
			</p>
      <hr v-if="msgs.length > 0">
      
      <div class="is-flex send-message">
        <input class="input" placeholder="Jot something down" v-model="text" @keyup.enter="sendMessage" />
        <button class="button is-primary is-outlined" @click="sendMessage">Send</button>
      </div>
		</div>
	</div>
</template>

<script>
import Web3 from "web3"
import SymmetricKeyConfig from "./SymmetricKeyConfig.vue"
import AsymmetricKeyConfig from "./AsymmetricKeyConfig.vue"
import {
  decodeFromHex,
  encodeToHex
} from "../utils/hexutils"
const defaultRecipientPubKey = "0x04ffb2647c10767095de83d45c7c0f780e483fb2221a1431cb97a5c61becd3c22938abfe83dd6706fc1154485b80bc8fcd94aea61bf19dd3206f37d55191b9a9c4"
const defaultTopic = "0x5a4ea131"

export default {
  data() {
    this.web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"))
    this.shh = this.web3.shh
    let data = {
      msgs: [],
      text: "",
      name: "",
      asymKeyId: null,
      asymPubKey: "",
      symKeyId: null,
      symPassword: "",
      asym: true,
      configured: false,
      topic: defaultTopic,
      recipientPubKey: defaultRecipientPubKey
    }
    this.shh.newKeyPair().then(id => {
      data.asymKeyId = id
      /* eslint-disable no-console */
      return this.shh.getPublicKey(id).then(pubKey => this.asymPubKey = pubKey).catch(console.log)
    }).catch()
    console.log(data)
    return data
  },
  components: {
    AsymmetricKeyConfig,
    SymmetricKeyConfig
  },
  methods: {
    sendMessage() {
      let msg = {
        text: this.text,
        name: this.name
      }
      this.msgs.push(msg)
      console.log(this.msgs)
      console.log(this.asym)
      let postData = {
        ttl: 7,
        topic: "0x07678231",
        powTarget: 2.01,
        powTime: 100,
        payload: encodeToHex(JSON.stringify(msg)),
      }
      if (this.asym) {
        postData.pubKey = this.recipientPubKey
        postData.sig = this.asymKeyId
      } else {
        postData.symKeyID = this.symKeyId
      }
      this.shh.post(postData)
      this.text = ""
    },
    updateSymKey(symPassword) {
      console.log(symPassword)
      this.shh.generateSymKeyFromPassword(symPassword).then(symKeyID => this.symKeyId = symKeyID)
      console.log("symKeyID: " + this.symKeyID)
    },
    configWithKey() {
      // TODO use a form
      if (!this.name || this.name.length == 0) {
        alert("Please pick a username")
        return
      }
      let filter = {
        topics: ["0xdeadbeef"]
      }
      if (this.asym) {
        if (!this.asymKeyId) {
          alert("No valid asymmetric key")
          return
        }
        filter.privateKeyID = this.asymKeyId
      } else {
        if (!this.symKeyId || this.symKeyId.length == 0) {
          alert("please enter a pasword to generate a key!")
          return
        }
        filter.symKeyID = this.symKeyId
      }
      this.msgFilter = this.shh.newMessageFilter(filter).then(filterId => {
        setInterval(() => {
          this.shh.getFilterMessages(filterId).then(messages => {
            for (let msg of messages) {
              let message = decodeFromHex(msg.payload)
              this.msgs.push({
                name: message.name,
                text: message.text
              })
            }
          })
        }, 1000)
      })
      this.configured = true
    }
  }
}
</script>

<style lang="scss">
.container {
  text-align: center;
}

.chat-messages {
  
}

.send-message {
  
}

.button-start {
  -moz-border-radius: 24px;
  -webkit-border-radius: 24px;
  border-radius: 24px;
  display: flex;
  align-items: center;
  padding: 16px 32px;
  margin: 0 auto 16px;
  -moz-background-clip: padding;
  -webkit-background-clip: padding-box;
  background-clip: padding-box;
  background: linear-gradient(164deg,#ffb046,#f52880) 0 100%;
    background-size: auto 150%;
  font-size: 18px;
  font-weight: 600;
  text-align: center;
  color: #fff;
  transition: all 0.3s;
  box-shadow: 0 8px 12px 0 rgba(0,0,0,.12), 0 2px 4px 0 rgba(0,0,0,.15);
	border: none;
  cursor: pointer;

  &:hover, &:active{
    color: #fffff;
		transform: perspective(1px) scale(1.05) translateZ(0);
		-webkit-transform: perspective(1px) scale(1.05) translateZ(0);
  }
}
</style>
