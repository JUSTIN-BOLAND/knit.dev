<template>
  <v-app-bar
    id="the-app-bar"
    v-scroll="onScroll"
    app
    :flat="$vuetify.breakpoint.smAndDown"
    :absolute="$vuetify.breakpoint.smAndDown"
    :elevate-on-scroll="$vuetify.breakpoint.mdAndUp"
    class="global-padded"
  >
    <v-row no-gutters>
      <v-col
        align="center"
        justify="space-between"
        class="d-flex align-center"
        md="4"
        lg="3"
      >
        <nuxt-link exact to="/">
          <v-img
            :src="siteLogo"
            height="2.5rem"
            width="2.5rem"
            alt="site logo"
          ></v-img>
        </nuxt-link>

        <v-toolbar-title class="ml-3 font-weight-bold">{{
          siteName
        }}</v-toolbar-title>
      </v-col>

      <v-col
        v-if="$vuetify.breakpoint.mdAndUp"
        class="d-flex align-center justify-space-around"
      >
        <TheAppBarTabs />
      </v-col>

      <v-col md="4" lg="3" class="d-flex justify-end">
        <template v-if="$vuetify.breakpoint.mdAndUp">
          <CallToActionButton v-show="showCallToActionButton" />
        </template>

        <v-app-bar-nav-icon
          class="hidden-md-and-up"
          aria-label="toggle navigation drawer"
          @click.stop="$emit('toggleDrawer')"
        ></v-app-bar-nav-icon
      ></v-col>
    </v-row>
  </v-app-bar>
</template>

<script lang="ts">
import { defineComponent, computed } from '@vue/composition-api'
import TheAppBarTabs from '~/components/TheAppBarTabs.vue'
import CallToActionButton from '~/components/CallToActionButton.vue'
import useCallToActionButton from '~/composables/useCallToActionButton'

export default defineComponent({
  components: {
    TheAppBarTabs,
    CallToActionButton
  },
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  setup(props, { root }) {
    const siteName = computed(() => root.$store.getters.getSiteName)
    const isDark = computed(() => root.$store.getters['theme/isDark'])
    const siteLogo = computed(() =>
      isDark.value
        ? require('~/assets/images/knit/knit-logo-white.svg')
        : require('~/assets/images/knit/knit-logo-black.svg')
    )

    const showCallToActionButton = computed(
      () => root.$store.getters.getShowCallToActionButton
    )
    const { setShowCallToActionButton } = useCallToActionButton(root)
    const onScroll = (e: any) => {
      if (typeof window === 'undefined') return
      const top = window.pageYOffset || e.target.scrollTop || 0

      const pageCallToAction = document.getElementById('page-call-to-action')
      if (pageCallToAction) {
        setShowCallToActionButton(
          top >
            pageCallToAction.getBoundingClientRect().top + window.pageYOffset
        )
      } else if (!showCallToActionButton.value) {
        setShowCallToActionButton(true)
      }
    }

    return {
      showCallToActionButton,
      onScroll,
      siteName,
      siteLogo
    }
  }
})
</script>

<style lang="scss" scoped>
@supports (
  (-webkit-backdrop-filter: blur(20px)) or (backdrop-filter: blur(20px))
) {
  #the-app-bar {
    background-color: transparent;
  }
  #the-app-bar ::v-deep > div:first-child {
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
  }
}

#the-app-bar.global-padded ::v-deep > div:first-child {
  padding-left: calc(var(--global-padding) + 1rem);
  padding-right: calc(var(--global-padding) + 1rem);
}
</style>
