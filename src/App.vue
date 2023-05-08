<template>
  <div class="container">
    <h1>Wallet Connect Örnek </h1>

    <div v-if="!connected">
      <button style="border:none; outline:none; border-radius:5px; background:aqua; color:#666; font-size:20px" @click="connectWallet">Connect Wallet</button>
    </div>

    <div v-else>
      <p>Wallet address: {{ address }}</p>
      <button @click="disconnectWallet">Disconnect Wallet</button>

      <div v-if="accounts.length > 0">
        <button  @click="sendTransaction">Send Transaction</button>
      </div>
    </div>

    <QrCodeModal v-if="showModal" :qr-code-data="qrCodeData" @close="showModal = false" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import WalletConnectProvider from '@walletconnect/web3-provider';
import Web3 from 'web3';
import QrCodeModal from './components/QrCodeModal.vue';

const connected = ref(false);
const address = ref('');
const accounts = ref([]);
const showModal = ref(false);
const qrCodeData = ref('');

async function connectWallet() {
  // Create a WalletConnectProvider instance
  const provider = new WalletConnectProvider({
    rpc: {
      1: 'https://mainnet.infura.io/v3/your-project-id',
      3: 'https://ropsten.infura.io/v3/your-project-id',
    },
  });

  // Enable session (triggers QR Code modal)
  await provider.enable();

  // Create Web3 instance
  const web3 = new Web3(provider);

  // Get accounts
  const accountsResult = await web3.eth.getAccounts();
  accounts.value = accountsResult;
  address.value = accountsResult[0];

  connected.value = true;
}

async function disconnectWallet() {
  // Reset data
  accounts.value = [];
  address.value = '';
  connected.value = false;

  // Close WalletConnect provider
  await provider.disconnect();
}

function sendTransaction() {
  // Show QR code modal
  qrCodeData.value = `ethereum:${address.value}`;
  showModal.value = true;
}
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;


  margin: 0 auto;
  text-align: center;
}
</style>
