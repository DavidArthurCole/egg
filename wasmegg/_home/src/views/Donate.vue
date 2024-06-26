<template>
  <h1 class="mb-4 text-center text-h1 leading-6 font-medium text-gray-900">Donate</h1>

  <div
    class="p-4 grid grid-cols-max-1 sm:grid-cols-max-2 gap-x-2 gap-y-0.5 items-center justify-center bg-green-50 rounded-lg shadow-inner"
  >
    <base-link href="https://ko-fi.com/carpetsage">
      <img :src="kofiIconUrl" class="h-8" :style="{ width: '3.94737rem' }" />
    </base-link>
    <base-link href="https://ko-fi.com/carpetsage" class="px-1">
      ko-fi.com/carpetsage
    </base-link>

    <div class="h-1 sm:hidden"></div>

    <base-link href="https://www.paypal.com/donate/?hosted_button_id=VVN47LKWKNAN2" class="px-1">
      <img :src="paypalIconUrl" class="h-5" :style="{ width: '3.94737rem' }" />
    </base-link>
    <base-link href="https://www.paypal.com/donate/?hosted_button_id=VVN47LKWKNAN2" class="px-1">
      paypal.com/donate
    </base-link>
  </div>

  <p>
    Donors will be credited here as well as on the home page.
    <strong>When you donate, please leave a note on how you want to be credited</strong>
    (or if you would rather stay anonymous)!
  </p>

  <p class="text-red-900">
    Sometimes the list of supporters is updated a few days to a few weeks late after your donation.
    I apologize in advance.
  </p>

  <p>
    Feel free to contact <span class="font-medium">CarpetSage#0100</span> on the
    <base-link href="https://discord.gg/egginc">Egg, Inc. Discord server</base-link>.
  </p>

  <h2 class="my-4 font-medium underline">Why am I calling for donations now</h2>

  <p>
    My public tools are hosted on <base-link href="https://netlify.com">Netlify</base-link>, which
    has a reasonably generous free tier. The site is <strong>currently not close to outgrowing the frier tier.</strong>
    However, the site will <strong>eventually</strong> outgrow the free tier as traffic steadily increases. 
    The bandwidth costs will be <strong>US$20 per month</strong> plus taxes.
  </p>

  <p>
    My tools also use
    <base-link href="https://workers.cloudflare.com/">Cloudflare Workers</base-link> for API
    requests; usage is comfortably within the free tier for now. The paid tier is <strong>US$5 per month</strong> 
    and provides some interesting options.
  </p>

  <p>
    Maintaining/Developing a broad range of tools is already time consuming, yet I am willing to do it for free
    and release the source code for everyone to study. However, I do not want to be on the hook for
    hosting bills as well, indefinitely.
  </p>

  <p>
    Therefore, if you like and regularly benefit from these tools,
    <strong>please kindly consider chipping in a little to keep the lights on.</strong>
  </p>

  <p>
    Before you donate, please keep in mind that
    <strong>your donations are entirely for services already rendered</strong>.
  </p>

  <p class="text-gray-500">
    For programmers reading this: you might be thinking, "I can host this for free / on a $5
    server". Yes, I can too if I put some effort into setting up a deployment pipeline, keeping the
    server patched, and dealing with possible downtime once in a while. But I would rather not touch
    the operations side unless necessary. I also would rather not settle for free hosts with
    arbitrary limitations, e.g. on caching headers.
  </p>

  <p class="text-gray-500">
    I might consider ways to further reduce costs if I cannot raise enough funds.
  </p>

  <h2 class="my-4 font-medium underline">Supporters</h2>

  <div>
    <p>The following supporters have my gratitude:</p>
    <donor-list />
  </div>

  <p><base-router-link :to="{ name: 'home' }">Back to home page</base-router-link></p>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import { setDonationPageVisited } from 'lib';
import BaseLink from '@/components/BaseLink.vue';
import BaseRouterLink from '@/components/BaseRouterLink.vue';
import DonorList from '@/components/DonorList.vue';

import kofiIconUrl from '@/brands/ko-fi.svg';
import paypalIconUrl from '@/brands/paypal.svg';

export default defineComponent({
  components: {
    BaseLink,
    BaseRouterLink,
    DonorList,
  },
  setup() {
    setDonationPageVisited();
    return {
      kofiIconUrl,
      paypalIconUrl,
    };
  },
});
</script>

<style lang="postcss" scoped>
p,
::v-deep(p) {
  @apply my-4;
}
</style>
