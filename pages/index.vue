<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">Mini taller de desarrollo de dApps sobre Substrate</h1>
      <b-table striped hover :items="items"></b-table>
    </div>
  </div>
</template>

<script>
import { ApiPromise, WsProvider } from '@polkadot/api'
export default {
  data() {
    return {
      items: [],
      api: undefined,
      unsubscribe: undefined,
    }
  },
  async created() {
    const nodeWs = 'wss://kusama-rpc.polkadot.io'
    const wsProvider = new WsProvider(nodeWs)
    this.api = await ApiPromise.create({ provider: wsProvider })
    this.unsubscribe = await this.api.rpc.chain.subscribeNewHeads((header) => {
      // eslint-disable-next-line
      console.log(`Chain is at block: #${header.number}`)
      this.items.push({ blockNumber: header.number })
    })
  },
  beforeDestroy: () => {
    this.unsubscribe()
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 60px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
