<template>
  <v-footer id="the-footer" padless>
    <v-card
      id="the-footer-card"
      flat
      tile
      width="100%"
      :color="isDark ? '' : '#F8F0EA'"
      class="text-center"
    >
      <v-card-text>
        <v-row justify="center">
          <v-col cols="12" class="d-flex align-self-center justify-center">
            <nuxt-link exact to="/">
              <v-img
                :src="siteLogo"
                height="2.5rem"
                width="2.5rem"
                alt="site logo"
              ></v-img>
            </nuxt-link>
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col cols="12">
            <div>
              <div class="font-weight-bold text-h5 pb-6">
                Contact us to discuss your project and how we can collaborate
              </div>
              <CallToActionButton />
            </div>
          </v-col>
        </v-row>
        <v-row>
          <v-col order="1" class="d-flex align-self-center justify-center">
            <ColorSchemeToggles />
          </v-col>
          <v-col
            cols="12"
            sm=""
            :order="$vuetify.breakpoint.xsOnly ? 'first' : 2"
            class="d-flex align-self-center flex-shrink-0 justify-center"
          >
            <div>
              <v-btn
                v-for="item in socials"
                :key="item.name"
                :href="item.link"
                target="_blank"
                rel="”noopener”"
                class="mx-2"
                icon
                :aria-label="item.name"
              >
                <v-icon>{{ item.icon }}</v-icon>
              </v-btn>
            </div>
          </v-col>
        </v-row>

        <v-row justify="center" no-gutters>
          <v-col col="12" md="10" lg="9" xl="8">
            <v-row>
              <v-col
                v-for="item in affiliates"
                :key="item.name"
                class="text-center"
              >
                <div>
                  <div class="affiliate-text">{{ item.title }}</div>
                  <a
                    :href="item.link"
                    target="_blank"
                    rel="”noopener”"
                    :aria-label="`${item.title} ${item.name}`"
                  >
                    <v-img
                      :src="
                        item.darkImage && isDark ? item.darkImage : item.image
                      "
                      :alt="`${item.name}-logo-and-text`"
                      aspect-ratio="1"
                      contain
                      height="1.375rem"
                      class="affiliate-image"
                      :class="{ 'affiliate-image-dark': isDark }"
                    />
                  </a>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-card-text>
      <v-divider></v-divider>
      <v-card-text>
        <div>
          &copy; {{ new Date().getFullYear() }} {{ companyName }}.
          {{ license.description }}
          <a :href="license.link" target="_blank" rel="”noopener”">{{
            license.linkText
          }}</a>
        </div>
        <v-row no-gutters justify="center" class="text-center">
          <v-col class="d-flex justify-end"
            ><nuxt-link exact to="/privacy" class="text-decoration-none"
              >Privacy</nuxt-link
            ></v-col
          >
          <div class="mx-2">•</div>
          <nuxt-link exact to="/terms" class="text-decoration-none"
            >Terms of Use</nuxt-link
          >
          <div class="mx-2">•</div>
          <v-col class="d-flex justify-start"
            ><a
              href="https://angry-goldberg-c2fd92.netlify.app/"
              class="text-decoration-none"
              >Old Site</a
            ></v-col
          >
        </v-row>
      </v-card-text>
    </v-card>
    <div
      v-for="item in footerFloatingIcons"
      :key="item.image"
      class="test-icon justify-center align-center d-none d-lg-flex"
      :style="item.style"
    >
      <v-img :src="item.image"></v-img>
    </div>
  </v-footer>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from '@vue/composition-api'
import {
  socials as socialsData,
  affiliates as affiliatesData,
  license as licenseData,
  footerFloatingIcons as footerFloatingIconsData
} from '~/data'
import ColorSchemeToggles from '~/components/ColorSchemeToggles.vue'
import CallToActionButton from '~/components/CallToActionButton.vue'

export default defineComponent({
  components: {
    ColorSchemeToggles,
    CallToActionButton
  },
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  setup(props, { root }) {
    const companyName = computed(() => root.$store.getters.getCompanyName)
    const isDark = computed(() => root.$store.getters['theme/isDark'])
    const siteLogo = computed(() =>
      isDark.value
        ? require('~/assets/images/knit/knit-logo-white.svg')
        : require('~/assets/images/knit/knit-logo-black.svg')
    )
    const socials = ref(socialsData)
    const affiliates = ref(affiliatesData)
    const footerFloatingIcons = ref(footerFloatingIconsData)
    const license = ref(licenseData)

    return {
      isDark,
      companyName,
      siteLogo,
      socials,
      affiliates,
      license,
      footerFloatingIcons
    }
  }
})
</script>

<style lang="scss" scoped>
@supports (padding-bottom: env(safe-area-inset-bottom)) {
  #the-footer-card {
    padding-bottom: env(safe-area-inset-bottom);
  }
}

$icon-size: 4rem;
.test-icon {
  position: absolute;
  padding: 3px;
  width: $icon-size;
  height: $icon-size;
  box-shadow: 8px 8px 15px rgba(19, 6, 65, 0.1);
  border-radius: 10px;
  z-index: 4;
  background: #ffffff;
}

.affiliate-text {
  margin-bottom: 0.5rem;
  color: rgb(153, 153, 153);
}
.affiliate-image {
  filter: grayscale(100%) contrast(40%);
  transition-duration: 150ms;
  transition-timing-function: ease-out;
  transition-delay: initial;
  transition-property: all;
}
.affiliate-image:hover {
  filter: grayscale(0%);
}
.affiliate-image-dark {
  filter: grayscale(100%) contrast(10%);
}
</style>
