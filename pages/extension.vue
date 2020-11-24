<template>
  <b-container class="py-5">
    <h1 class="mb-4">Extension addresses</h1>
    <div v-if="!detectedExtension">
      <b-alert variant="warning" show>
        <i class="fa fa-frown-o"></i>
        <a href="https://github.com/polkadot-js/extension" target="_blank"
          >Polkadot JS extension</a
        >
        not found, please install it and import your account/s before proceed
      </b-alert>
    </div>
    <div v-else-if="!extensionAccounts">
      <b-alert variant="warning" show>
        <i class="fa fa-frown-o"></i> No accounts found, open Polkadot JS
        extension and import your account/s before proceed
      </b-alert>
    </div>
    <div v-else>
      <pre>{{ extensionAccounts }}</pre>
    </div>
  </b-container>
</template>

<script>
import { web3Accounts, web3Enable } from '@polkadot/extension-dapp'
import { ApiPromise, WsProvider } from '@polkadot/api'

export default {
  data() {
    return {
      addressPrefix: 2,
      detectedExtension: false,
      extensionAccounts: [],
      extensionAddresses: [],
      selectedAccount: null,
      selectedAddress: null,
      tranferableBalance: 0,
      api: null,
      enableWeb3: false,
      error: null,
      amount: 0,
      extrinsicHash: null,
      extrinsicStatus: null,
      blockHash: null,
      success: null,
      noAccountsFound: true,
      addressRole: null,
      onGoingElection: false,
    }
  },
  async created() {
    this.enableWeb3 = await web3Enable(`My awesome dApp`)
      .then(() => {
        web3Accounts()
          .then((accounts) => {
            const wsProvider = new WsProvider('wss://kusama-rpc.polkadot.io')
            ApiPromise.create({ provider: wsProvider }).then((api) => {
              this.api = api
              if (accounts.length > 0) {
                this.detectedExtension = true
                this.extensionAccounts = accounts
              }
            })
          })
          .catch((error) => {
            // eslint-disable-next-line
            console.log('Error: ', error)
          })
      })
      .catch((error) => {
        // eslint-disable-next-line
        console.log('Error: ', error)
      })
  },
}
</script>
