<script setup>
import CryptoJS from 'crypto-js'
import {ref, watch} from "vue";

const encrypt = ref(true)
const secretCode = ref('')
const codeEncrypt = ref('')
const codeDecrypt = ref('')

watch(codeEncrypt, () => {
  if (encrypt.value && !codeEncrypt.value) {
    codeDecrypt.value = ''
  }
})

watch(codeDecrypt, () => {
  if (!encrypt.value && !codeDecrypt.value) {
    codeEncrypt.value = ''
  }
})

watch(secretCode, () => {
  codeEncrypt.value = ''
  codeDecrypt.value = ''
})

function handleClick () {
  if (encrypt.value) {
    handleEncrypt()
  } else {
    handleDecrypt()
  }
}
function handleEncrypt () {
  if (codeEncrypt.value && secretCode.value) {
    const encrypted = CryptoJS.AES.encrypt(codeEncrypt.value, secretCode.value);
    codeDecrypt.value = encrypted.toString();
  }
}
function handleDecrypt () {
  if (codeDecrypt.value && secretCode.value) {
    const bytes  = CryptoJS.AES.decrypt(codeDecrypt.value, secretCode.value);
    codeEncrypt.value = bytes.toString(CryptoJS.enc.Utf8);
  }
}

</script>

<template>
  <main>
    <h1>Secret Code</h1>
    <input type="text" class="input" v-model="secretCode" placeholder="Input secret code">
    <div v-if="encrypt">
      <h2>Encrypt</h2>
      <textarea id="1" cols="30" rows="10" class="input" v-model="codeEncrypt"></textarea>
    </div>
    <div v-else>
      <h2>Decrypt</h2>
      <textarea id="2" cols="30" rows="10" class="input" v-model="codeDecrypt"></textarea>
    </div>
    <div>
      <button class="button button-primary" @click="encrypt = !encrypt">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M16.707 10.293a1 1 0 010 1.414l-6 6a1 1 0 01-1.414 0l-6-6a1 1 0 111.414-1.414L9 14.586V3a1 1 0 012 0v11.586l4.293-4.293a1 1 0 011.414 0z" clip-rule="evenodd" />
        </svg>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M5.293 7.707a1 1 0 010-1.414l4-4a1 1 0 011.414 0l4 4a1 1 0 01-1.414 1.414L11 5.414V17a1 1 0 11-2 0V5.414L6.707 7.707a1 1 0 01-1.414 0z" clip-rule="evenodd" />
        </svg>
      </button>
    </div>
    <div v-if="encrypt">
      <h2>Decrypt</h2>
      <textarea disabled id="3" cols="30" rows="10" class="input" v-model="codeDecrypt"></textarea>
    </div>
    <div v-else>
      <h2>Encrypt</h2>
      <textarea disabled id="4" cols="30" rows="10" class="input" v-model="codeEncrypt"></textarea>
    </div>
    <button type="submit" class="button button-success" :disabled="!secretCode" @click="handleClick">Submit</button>
  </main>
</template>

<style>
.w-5 {
  width: 18px;
}
</style>
