<template>
  <div v-if="false" />
</template>
<script lang="ts">
import Vue from 'vue'
import Noty from 'noty'
import { mapGetters, mapActions, mapState } from 'vuex'
import logoUrl from '@/assets/logo.svg'

export default Vue.extend({
  data: () => {
    return {
      notificationInterval: null,
      timeoutID: null,
      isShowingOnboardingNoty: false,
      upsellNotificationOptions: {
        text: "Upgrade for features like the JSON row viewer, AI shell, & NoSQL support. All purchases come with a <strong>lifetime usage license</strong>.",
        timeout: 1000 * 60 * 5,
        queue: "upsell",
        killer: 'upsell',
        layout: 'bottomRight',
        closeWith: ['button'],
        buttons: [
          Noty.button('Close', 'btn btn-flat', () => Noty.closeAll('upsell')),
          Noty.button('Get Started', 'btn btn-primary', () => window.main.openExternally('https://beekeeperstudio.io/pricing/'))
        ]
      },
      onboardingNoty: null as Noty | null,
    }
  },
  computed: {
    ...mapGetters({
      'isCommunity': 'isCommunity',
    }),
    ...mapGetters(['onboardingNotyShown', 'connected']),
    ...mapState(['connected']),
  },
  watch: {
    isCommunity() {
      this.initNotifyInterval()
    },
    connected() {
      if (this.connected && !this.onboardingNotyShown) {
        this.setOnboardingNotyShown()
      }
      this.noty?.close();
    },
  },
  methods: {
    ...mapActions(['setOnboardingNotyShown']),
    initNotifyInterval() {
      return;
    }
  },
  mounted() {
    this.initNotifyInterval()
    this.notifyOnboarding()
  }
})
</script>
