<script setup>
import QRGenerator from "../components/QRGenerator.vue";
</script>

<template>
  <main>


    <div class="container">

      <div class="w-[700px] max-w-full mx-auto text-center mt-12">

        <div class="intro">
          <h2 class="text-xl font-bold mb-3">Your easiest way to create your QR Code</h2>
          <p class="text-sm">This tool was created to easily generate a QR code and download it in a format you can use to print, not a small and blurred png file!</p>

        </div>

        <div class="qr my-6">
          <QRGenerator></QRGenerator>
        </div>


      </div>

    </div>

  </main>
</template>
