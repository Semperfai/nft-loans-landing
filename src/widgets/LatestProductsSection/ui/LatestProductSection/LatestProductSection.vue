<script setup lang="ts">
import { BaseButton } from '@/shared/ui'
import { Spinner } from '@/shared/ui'
import { useBreakpoints } from '@/shared/lib/composables/useBreakpoints'
import { VideoViewerButton } from '@/features/video/videoViewer'
import { VideoViewerPlayer } from '@/features/video/videoViewer'
import LatestProductCard from '../LatestProductCard/LatestProductCard.vue'
import LatestProductSectionLayout from '../LatestProductSectionLayout/LatestProductSectionLayout.vue'
import { ref } from 'vue'
import { onClickOutside } from '@vueuse/core'

const show = ref(false)
const videoViewerPlayerRef = ref(null)

const breakpoints = useBreakpoints()

onClickOutside(videoViewerPlayerRef, () => {
  show.value = !show.value
})

const playVideo = () => {
  show.value = !show.value
}
</script>
<template>
  <section class="py-16">
    <LatestProductSectionLayout>
      <template #textContent>
        <h1
          class="lg:mb-8 mb-5 lg:text-6xl sm:text-3xl text-2xl font-semibold [text-shadow:_0_4px_4px_rgb(0_0_0_/_40%)]"
        >
          Discover Our Latest Products
        </h1>
        <p class="lg:mb-14 mb-8 lg:text-2xl md:text-xl text-sm">
          Lorem ipsum is a placeholder text commonly used to demonstrate the visual form of a
          product
        </p>
      </template>
      <template #action>
        <BaseButton
          :size="breakpoints.isLg ? 'large' : breakpoints.isSm ? 'medium' : 'small'"
          class="lg:mb-28 md:mb-20 mb-14 rounded-xl"
          type="primary"
          text="Buy Now"
        />
      </template>
      <template #productContent>
        <VideoViewerButton
          v-if="!show"
          @click="playVideo"
          class="lg:mb-14 sm:flex md:static absolute right-0 top-0 z-10"
        />
        <LatestProductCard />
      </template>
    </LatestProductSectionLayout>
    <template v-if="show">
      <Transition mode="out-in">
        <Suspense>
          <component ref="videoViewerPlayerRef" :is="VideoViewerPlayer" />
          <template #fallback>
            <div class="fixed grid justify-center content-center inset-0 bg-black opacity-75">
              <Spinner />
            </div>
          </template>
        </Suspense>
      </Transition>
    </template>
  </section>
</template>
