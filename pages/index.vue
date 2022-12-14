<template>
  <div class="h-screen overflow-x-hidden overflow-y-scroll">
    <div ref="top" class="invisible" />

    <!-- FAB -->
    <Transition enter-from-class="translate-y-4 opacity-0" enter-to-class="translate-y-0"
      enter-active-class="transition" leave-to-class="translate-y-4 opacity-0" leave-active-class="transition">
      <NuxtLink v-show="showFAB" to="/timer"
        class="fixed bottom-0 z-10 w-full transition duration-300 xl:w-auto xl:right-6 xl:bottom-4">
        <button type="button" tabindex="0"
          class="flex flex-row items-center w-full px-4 py-3 space-x-2 text-xl font-bold text-black uppercase transition border rounded-t-lg shadow-md cursor-pointer select-none xl:rounded-lg bg-amber-300 hover:bg-amber-400 shadow-amber-300/30 hover:shadow-amber-300/60 active:duration-500 active:shadow-xl active:shadow-amber-300/80 active:bg-amber-500 border-neutral-100">
          <img src="/favicon.svg" width="32" height="32" role="presentation">
          <span v-text="$t('index.launch')" />
        </button>
      </NuxtLink>
    </Transition>

    <!-- Section 1: intro -->

    <Section ref="section-1"
      class="h-[95vh] flex flex-col items-center justify-center px-4 overflow-hidden bg-gray-100">
      <Columns class="flex-col gap-8 px-4 pt-8 xl:flex-row xl:space-x-16 xl:px-24 xl:pt-0">
        <template #left>
          <!-- App title and CTAs -->
          <div class="flex flex-col justify-center transition-all duration-1000"
            :class="{ 'opacity-0 -translate-x-4': !loading.mainText }">
            <div class="flex flex-col items-start gap-2 xl:mr-16 md:flex-row md:gap-4">
              <!-- App icon -->
              <div class="aspect-square min-w-fit md:mt-1">
                <img src="/favicon.svg" width="68" height="68" class="p-2 bg-red-200 rounded-lg" role="presentation">
              </div>
              <!-- App name and slogan -->
              <div class="flex flex-col">
                <h1 class="text-3xl font-bold md:text-5xl">
                  TimeTrackingApp
                </h1>
                <div class="text-lg md:text-xl" v-text="$t('index.app_description')" />
              </div>
            </div>
            <!-- CTAs -->
            <div class="grid grid-flow-row grid-cols-1 gap-2 mt-6 md:grid-flow-col">
              <nuxt-link to="/timer">
                <button type="button"
                  class="flex-grow w-full px-6 py-4 text-2xl font-bold text-center uppercase transition-all rounded-lg shadow-lg cursor-pointer select-none bg-amber-300 hover:bg-amber-400 shadow-amber-300/30 hover:shadow-amber-300/60 active:duration-500 active:shadow-xl active:shadow-amber-300/80 active:bg-amber-500"
                  role="button" tabindex="0" v-text="$t('index.cta.quickstart')" />
              </nuxt-link>
              <!-- <nuxt-link v-slot="{ navigate }" to="/setup" custom>
                <div class="flex-grow px-6 py-4 text-2xl font-bold text-center uppercase transition-all rounded-lg cursor-pointer select-none bg-slate-300 hover:bg-gray-300 shadow-slate-300/0 hover:shadow-slate-300/40 hover:shadow-lg active:shadow-slate-300/60 active:bg-slate-400" role="button" tabindex="0" @click="navigate" v-text="$t('index.cta.configure')" />
              </nuxt-link> -->
            </div>
          </div>
        </template>
        <!-- App screenshot -->
        <template #right>
          <div
            :class="['transition-all duration-1000 hover:duration-300 overflow-hidden order-first xl:order-last rounded-lg shadow-red-300/60 shadow-lg hover:-translate-y-1 hover:shadow-xl hover:shadow-red-300/70 w-full max-w-2xl xl:max-w-3xl self-center', { 'opacity-0 translate-x-4': !loading.screenshot }]">
            <img :alt="$t('index.alt.img.screenshot')" class="" width="1600" height="900"
              src="/assets/img/screenshots/720p/Timer_Default_2x.png" format="jpg" sizes="sm:760px xl:1600px"
              quality="80" fit="contain">
          </div>
        </template>
      </Columns>

      <template #after>
        <div class="flex flex-col items-center justify-end flex-grow mb-8 xl:absolute xl:bottom-4">
          <!-- Scroll indicator -->
          <ScrollIcon size="42" />
        </div>
      </template>
    </Section>

    <!-- Section 2: about Pomodoro and the app -->
    <Section ref="section-2" class="flex flex-col justify-center px-4 overflow-hidden bg-sky-100 py-36">
      <div class="flex flex-col items-center text-sky-900">
        <h2 class="text-5xl font-bold tracking-tight uppercase" v-text="$t('index.section_whatitdoes.title')" />
        <div class="mt-2 text-lg text-center xl:text-xl">
          <p v-text="$t('index.section_whatitdoes.subtitle.main', { appname: 'TimeTrackingApp' })" />
          <p v-text="$t('index.section_whatitdoes.subtitle.sub')" />
        </div>

        <!-- Schedule display imitation -->
        <div class="flex flex-row p-3 mt-16 space-x-3 rounded-lg shadow-lg bg-slate-800 shadow-slate-500/20">
          <div
            :class="['transition duration-500 ring-2 ring-inset ring-transparent w-12 h-12 bg-work rounded-lg', { '!ring-white': section2selectedBox === 1 }]" />
          <div
            :class="['transition duration-500 ring-2 ring-inset ring-transparent w-12 h-12 bg-shortpause rounded-lg', { '!ring-white': section2selectedBox === 2 }]" />
          <div
            :class="['transition duration-500 ring-2 ring-inset ring-transparent w-12 h-12 bg-longpause rounded-lg', { '!ring-white': section2selectedBox === 3 }]" />
        </div>

        <!-- Schedule cards -->
        <div class="grid items-start grid-flow-row gap-4 mt-4 xl:grid-flow-col xl:auto-cols-auto">
          <ScheduleCard :title="$t('index.section_whatitdoes.cards[0].title')"
            :description="$t('index.section_whatitdoes.cards[0].description')"
            :duration="$t('index.section_whatitdoes.cards[0].duration')" dot-class="bg-work"
            :class="[{ 'shadow-lg shadow-slate-700/30 !bg-slate-600': section2selectedBox === 1 }]" />

          <IconNext class="self-center hidden xl:block" />

          <ScheduleCard :title="$t('index.section_whatitdoes.cards[1].title')"
            :description="$t('index.section_whatitdoes.cards[1].description')"
            :duration="$t('index.section_whatitdoes.cards[1].duration')" dot-class="bg-shortpause"
            :class="[{ 'shadow-lg shadow-slate-700/30 !bg-slate-600': section2selectedBox === 2 }]" />

          <IconNext class="self-center hidden xl:block" />
          <IconDots class="self-center hidden xl:block" />
          <IconNext class="self-center hidden xl:block" />

          <ScheduleCard :title="$t('index.section_whatitdoes.cards[2].title')"
            :description="$t('index.section_whatitdoes.cards[2].description')"
            :duration="$t('index.section_whatitdoes.cards[2].duration')" dot-class="bg-longpause"
            :class="[{ 'shadow-lg shadow-slate-700/30 !bg-slate-600': section2selectedBox === 3 }]" />
        </div>
      </div>
    </Section>

    <!-- Section 3: Features -->
    <Section ref="section-3"
      class="flex flex-col items-center justify-start px-4 overflow-hidden text-center bg-amber-50 md:justify-center py-36">
      <h2 class="text-5xl font-bold tracking-tight uppercase text-amber-900"
        v-text="$t('index.section_features.title')" />

      <div class="grid max-w-5xl grid-flow-row grid-cols-2 gap-4 mt-8 text-lg xl:text-xl xl:grid-cols-4 xl:gap-8">
        <div v-for="(feature, index) in section3.smallFeatures" :key="feature"
          :class="['transition duration-1000 py-4 px-4 rounded-lg text-gray-900 text-opacity-80 flex flex-col justify-center', { '!text-gray-100 text-opacity-100 bg-slate-800': section3.activeFeature === index }]"
          v-text="$t('index.section_features.list.' + feature)" />
      </div>
    </Section>

  </div>
</template>

<script>
import { ChevronDownIcon, ChevronRightIcon, DotsIcon, BrandFacebookIcon, BrandTwitterIcon, BrandRedditIcon } from 'vue-tabler-icons'
import { mapState } from 'pinia'
import Columns from '@/components/index/columns.vue'
import Section from '@/components/index/section.vue'
import ScheduleCard from '@/components/index/scheduleCard.vue'

import SupportButton from '~/components/socialButtons/supportButton.vue'
import { useMain } from '~~/stores/main'

export default {
  name: 'PageHome',

  components: { Columns, Section, ScheduleCard, ScrollIcon: ChevronDownIcon, IconNext: ChevronRightIcon, IconDots: DotsIcon, SupportButton, AboutFacebook: BrandFacebookIcon, AboutTwitter: BrandTwitterIcon, AboutReddit: BrandRedditIcon },

  data() {
    return {
      loading: {
        mainText: false,
        screenshot: false
      },
      scroll: {
        fabObserver: null
      },
      section2: {
        selectedBoxIndex: 0,
        selectedBoxOrder: [1, 2, 1, 2, 1, 3],
        selectedBoxInterval: null
      },
      section3: {
        smallFeatures: [
          'customization', 'notifications', 'flexible', 'pwa', 'opensource', 'notrackers', 'noads',
          'clean', 'adaptiveticking', 'localization', 'darkmode', 'more'
        ],
        activeFeature: 4,
        activeFeatureInterval: null
      },
      faq: [
        { q: 'change_timers' },
        { q: 'will_it_help', hint: true },
        { q: 'data_collection' },
        { q: 'remember_settings', hint: true },
        { q: 'need_to_know' },
        { q: 'timer_style' }
      ],
      openfaq: 0,
      showFAB: false
    }
  },

  head() {
    return {
      title: 'TimeTrackingApp',
      link: [
        { rel: 'icon', href: '/favicon.svg' }
      ]
    }
  },

  computed: {
    section2selectedBox() {
      return this.section2.selectedBoxOrder[this.section2.selectedBoxIndex]
    },
    ...mapState(useMain, ['version'])
  },

  mounted() {
    this.$nextTick(() => {
      // Float in the app title and icon
      this.loading.mainText = true

      setTimeout(() => {
        // Float in the app screenshot
        this.loading.screenshot = true
      }, 1000)
    })

    // start animations
    this.selectedBoxInterval = setInterval(() => {
      this.section2.selectedBoxIndex = this.section2.selectedBoxIndex > 4 ? 0 : this.section2.selectedBoxIndex + 1
    }, 4000)

    this.activeFeatureInterval = setInterval(() => {
      this.section3.activeFeature = Math.floor(Math.random() * (this.section3.smallFeatures.length - 1))
    }, 3000)

    // register FAB scroll observer
    this.scroll.fabObserver = new IntersectionObserver(
      (entries) => {
        this.showFAB = !entries[0].isIntersecting
      },
      {
        root: this.$el,
        threshold: 1.0
      }
    ).observe(this.$refs.top)
  },

  beforeDestroy() {
    clearInterval(this.selectedBoxInterval)
    clearInterval(this.activeFeatureInterval)
    if (this.scroll.fabObserver) {
      this.scroll.fabObserver.disconnect()
    }
  },

  methods: {
    setIntersecting(value) {
      this.showFAB = value
    }
  }
}
</script>

<style lang="postcss" scoped>
.main-appear-enter-active {
  @apply transition duration-1000;
}

.main-appear-enter-from {
  @apply opacity-0 -translate-x-4;
}
</style>
