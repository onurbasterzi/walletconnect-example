<template>
  <div class="modal">
    <div class="modal-content">
      <div class="close" @click="closeModal">&times;</div>
      <canvas class="qr-code" ref="canvas"></canvas>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import QRCode from 'qrcode';

const qrCodeData = ref('');
const canvasRef = ref(null);

onMounted(() => {
  QRCode.toCanvas(canvasRef.value, qrCodeData.value);
});

const closeModal = () => {
  emit('close');
};
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fefefe;
  padding: 20px;
  border-radius: 10px;
}

.close {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
}

.qr-code {
  width: 200px;
  height: 200px;
}
</style>