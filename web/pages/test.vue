<template>
  <div>
    <button @click="clicked">Hello</button>
  </div>
</template>
<script>
import {
  wifi,
  wine,
  download,
  add,
  share,
  logoFacebook,
  logoInstagram,
  logoTwitter
} from 'ionicons/icons'
import { Browser } from '@capacitor/browser'
export default {
  data: () => {
    return {
      networkStatus: null,
      i: {
        wifi,
        wine,
        download,
        add,
        share,
        logoTwitter,
        logoInstagram,
        logoFacebook
      },
      password: 'message'
    }
  },
  mounted() {
    // check initial network and enable the listener for changes
    this.checkNetwork()
    this.$network.addListener('networkStatusChange', (status) => {
      this.networkStatus = status.connected ? 'online' : 'offline'
    })
  },
  methods: {
    async checkNetwork() {
      this.networkStatus = (await this.$network.getStatus()).connected
        ? 'online'
        : 'offline'
    },
    showToast() {
      this.$toast.show({ text: 'I am a toast!' })
    },
    async clicked() {
      await Browser.open({ url: 'http://capacitorjs.com/' })
    },
    installApp() {
      // Show the install promp()
      this.$store.state.deferredPrompt.prompt()
      // Wait for the user to respond to the prompt
      this.$store.state.deferredPrompt.userChoice.then((choiceResult) => {
        if (choiceResult.outcome === 'accepted') {
          this.$toast.show('Installation started!')
        } else {
          this.$toast.show('Installation canceled!')
        }
      })
    }
  }
}
</script>
<style scoped>
ion-item {
  cursor: pointer;
}
</style>
