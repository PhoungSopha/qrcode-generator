<script setup>
import QRCode from "./components/qrcode.vue"
import QRcode from "qrcode"
</script>

<template>
  <div><QRCode></QRCode></div>
</template>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
