<template>
  <section>
    <v-container fluid class="pa-0">
      <v-row no-gutters class="global-x-padding pb-4">
        <v-col cols="12" md="6" align-self="center">
          <div class="mx-auto pa-4" style="max-width: 700px;">
            <PageHeading :text="heading" />
            <p class="pb-6 text--secondary text-h6 font-weight-regular">
              Tell us about your project, find out if we're a good fit and
              receive an estimate. We're excited to discover new startup
              partners!
            </p>

            <v-row class="pb-6">
              <v-col cols="12" sm="6">
                <HeadingSubtitle text="Email" class="pb-2" />
                <div>
                  <v-icon large class="mr-2">{{ emailIcon }}</v-icon>
                  <a
                    class="text-body-1 font-weight-medium text-decoration-none"
                    :href="`mailto:${callToAction.email}`"
                    >{{ callToAction.email }}</a
                  >
                </div>
              </v-col>
              <v-col cols="12" sm="6">
                <HeadingSubtitle text="Headquarters" class="pb-2" />
                <address
                  class="text-body-1 font-weight-medium"
                  style="font-style: normal;"
                >
                  Knit LLC<br />
                  1724 Barry Ave<br />
                  Apt 8<br />
                  Los Angeles, CA 90025<br />
                  United States
                </address>
              </v-col>
            </v-row>

            <div class="d-flex align-self-center pb-4">
              <v-btn
                v-for="item in socials"
                :key="item.name"
                :href="item.link"
                target="_blank"
                rel="”noopener”"
                class="mr-2"
                icon
                large
                :aria-label="item.name"
              >
                <v-icon large>{{ item.icon }}</v-icon>
              </v-btn>
            </div>
          </div>
        </v-col>
        <v-col cols="12" md="6" class="pa-0">
          <div>
            <v-img src="" lazy-src="" alt="headquarters map"></v-img>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>

<script>
import { defineComponent, ref } from '@vue/composition-api'
import { mdiEmailOutline } from '@mdi/js'
import {
  callToAction as callToActionData,
  socials as socialsData
} from '~/data'
import HeadingSubtitle from '~/components/HeadingSubtitle.vue'
import PageHeading from '~/components/PageHeading.vue'
import useCallToActionButton from '~/composables/useCallToActionButton'

export default defineComponent({
  name: 'Contact',
  head() {
    const description =
      'Contact Knit to see if we can help fulfill your software development needs'
    return {
      title: "Let's chat",
      meta: [
        {
          name: 'og:description',
          hid: 'og:description',
          content: description
        },
        {
          name: 'description',
          hid: 'description',
          content: description
        }
      ]
    }
  },
  components: {
    HeadingSubtitle,
    PageHeading
  },
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  setup(props, { root }) {
    useCallToActionButton(root)

    const heading = ref("Let's chat")
    const emailIcon = ref(mdiEmailOutline)
    const callToAction = ref(callToActionData)
    const socials = ref(socialsData)

    return {
      heading,
      emailIcon,
      callToAction,
      socials
    }
  }
})
</script>

<style lang="scss" scoped></style>
