<script setup>
import {toString} from "qrcode"
import {saveAs} from "file-saver";
import {reactive, ref, watch} from "vue";


const qr = reactive({
  url: '',
  svg_output: '',
  url_input: '',
  url_input_timeout: null,
  color_timeout: null
});
const color = ref('#000000');

watch(color, () => {

  if(qr.color_timeout) clearTimeout(qr.color_timeout);

  qr.color_timeout = setTimeout(() => {
    generateQr()
  }, 250);

});
const generateQr = () => {
  qr.url = qr.url.trim(); // trim spaces

  if(!qr.url){
    qr.output = null;
    return;
  }
  toString(qr.url, { color: {dark: color.value,  light: '#0000'} },(err, string) => {
    if (err) throw err;
    qr.output = string;
  })
}

const getLink = (url) => {
  return url.includes('http://') || url.includes('https://') ? url : 'https://'+url
}

const downloadSvg = () => {
  const blob = new Blob([qr.output], {type: 'image/svg+xml'});
  saveAs(blob, qr.url + ".svg");
}


const debounceSearch = () => {
  if(qr.url_input_timeout){
    clearTimeout(qr.url_input_timeout);
  }
  qr.url_input_timeout = setTimeout(() => {
    qr.url = qr.url_input; generateQr()
  }, 400);
}
</script>


<style>
.viewer svg{
  @apply w-[350px] mx-auto;
}
</style>
<template>


  <div class="">
      <div class="mb-6">
        <div class="flex">

          <input v-model="qr.url_input" type="text" id="url" class="w-full" @input="debounceSearch" placeholder="https://manolisliolios@github.io/easy-qr">

          <div class="h-full ml-2 flex items-center">
            <input v-model="color" type="text" id="qr-color-val" class="w-[120px]" name="qr-color-val"/>

            <div class="h-[25px] w-[25px] rounded-[50px] overflow-hidden -ml-8">
              <input v-model="color" type="color" id="qr-color" class="h-[200%] p-0 m-0 w-[200%]" style="transform: translate(-25%, -25%)" name="qr-color"/>
            </div>
          </div>
        </div>
      </div>



    <div v-if="qr.output" class="text-center">

      <div class="grid md:grid-cols-2 items-center">
        <div v-html="qr.output" class="viewer mx-auto"></div>

        <div>
          <p class="text-sm mb-3">The generated QR links to <a :href="getLink(qr.url)" target="_blank">{{ qr.url }}</a><br/>Please verify it works before printing!</p>
          <button @click="downloadSvg" class="flex items-center mx-auto text-sm bg-gray-800 text-white px-3 py-2 rounded-lg">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                 stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-2">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
              <polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
            Download SVG
          </button>
        </div>
      </div>



    </div>


  </div>
</template>