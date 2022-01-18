<script setup>
import QRcode from "qrcode"
import { ref, computed } from "vue"
const inputUrl = ref("")
const QRImage = ref([])

function generateQRCode() {
  QRcode.toDataURL(inputUrl.value, { errorCorrectionLevel: "H" }, (err, url) => {
    if (err) throw err
    QRImage.value = url
  })
}
function downloadQRCode(source) {
  const link = document.createElement("a")
  link.setAttribute("href", source)
  link.setAttribute("download", "QRCode.png")
  link.style.display = "none"
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}
const shouldDisabledDownload = computed(() => {
  if (QRImage.value.length === 0) {
    return true
  } else {
    return false
  }
})

const shouldDisabledGenerate = computed(() => {
  if (inputUrl.value === "") {
    return true
  } else {
    return false
  }
})

const clearableInput = () => {
  inputUrl.value = ""
}
</script>
<template>
  <div class="fixed inset-0 flex items-center justify-center bg-blue-900 p-4">
    <div class="max-w-xl rounded-md bg-white p-10 w-full flex flex-col justify-center">
      <div class="relative">
        <input v-model="inputUrl" type="text" class="px-2 py-3 sm:pr-7 pr-8 w-full border-2 sm:text-sm rounded-md border-gray-200 outline-none" placeholder="Copy the url and past in here" /><button @click="clearableInput" v-show="inputUrl" class="absolute top-4 right-2 border rounded-full bg-gray-300">
          <svg xmlns="http://www.w3.org/2000/svg" class="sm:h-4 sm:w-4 h-5 w-5 fill-current text-gray-100" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </button>
      </div>
      <div class="flex justify-center items-center mt-4">
        <div class="border border-gray-200 w-44 h-44">
          <img :src="QRImage" alt="" />
        </div>
      </div>
      <div class="flex justify-center sm:gap-x-5 mt-5 flex-col sm:flex-row gap-y-2">
        <button :disabled="shouldDisabledGenerate" @click="generateQRCode" class="px-4 py-3 bg-blue-800 text-white font-semibold sm:text-sm rounded-sm active:ring-1 active:ring-blue-400 flex justify-center items-center gap-x-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="sm:h-5 sm:w-5 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" /></svg
          >Generate
        </button>
        <button :disabled="shouldDisabledDownload" @click="downloadQRCode(QRImage)" class="px-4 py-3 bg-blue-800 text-white font-semibold sm:text-sm rounded-sm active:ring-1 active:ring-blue-400 flex justify-center items-center gap-x-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="sm:h-5 sm:w-5 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" /></svg
          >Download
        </button>
      </div>
    </div>
  </div>
</template>
