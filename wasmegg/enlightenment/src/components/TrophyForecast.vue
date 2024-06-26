<template>
  <p v-if="lastRefreshedPopulation < targetPopulation" class="text-sm">
    <template v-if="habSpace >= targetPopulation">{{ trophyDisplayName }} forecast: </template>
    <template v-else>
      {{ trophyDisplayName }} forecast,
      <span
        v-tippy="{
          content: `This is NOT a given. Check the following sections for feasibility considerations.`,
        }"
        class="text-red-700 underline"
        >assuming sufficient hab space can be unlocked in time</span
      >:
    </template>
    <template v-if="completionForecast">
      <span class="text-green-500 whitespace-nowrap">
        {{ completionForecast.format('LLL z') }}
      </span>
      <template v-if="completionForecastDays !== null && completionForecastDays > 0">
        <template v-if="completionForecastDays > 1">
          ({{ completionForecastDays.toFixed(1) }} days)
        </template>
        <template v-else> (in {{ completionForecastFormattedDuration }})</template>
      </template>
    </template>
    <template v-else>Never</template>
  </p>
  <p
    v-if="
      trophyLevel === 'Diamond' &&
      completionForecastDays !== null &&
      completionForecastDays > 0 &&
      completionForecastDays < 1
    "
    class="text-base"
  >
    <span
      :style="{
        maskImage: `linear-gradient(
          90deg,
          rgba(0, 0, 0, 1) 0%,
          rgba(0, 0, 0, 1) ${completionForecastDays * 100}%,
          rgba(0, 0, 0, 0.25) ${completionForecastDays * 100}%,
          rgba(0, 0, 0, 0.25) 100%
        )`,
      }"
    >
      &#x1f90f;&#x1f90f;&#x1f3fb;&#x1f90f;&#x1f3fc;&#x1f90f;&#x1f3fd;&#x1f90f;&#x1f3fe;&#x1f90f;&#x1f3ff;
    </span>
  </p>
</template>

<script lang="ts">
import { computed, defineComponent, onBeforeUnmount, PropType, ref, toRefs } from 'vue';
import dayjs from 'dayjs';
import advancedFormat from 'dayjs/plugin/advancedFormat';
import localizedFormat from 'dayjs/plugin/localizedFormat';
import relativeTime from 'dayjs/plugin/relativeTime';
import timezone from 'dayjs/plugin/timezone';
import utc from 'dayjs/plugin/utc';

import { formatDuration } from '@/lib';

// Note that timezone abbreviation may not work due to
// https://github.com/iamkun/dayjs/issues/1154, in which case the GMT offset is
// shown.
dayjs.extend(advancedFormat);
dayjs.extend(localizedFormat);
dayjs.extend(relativeTime);
dayjs.extend(timezone);
dayjs.extend(utc);

export default defineComponent({
  name: 'TrophyForecast',
  props: {
    trophyLevel: {
      type: String,
      required: true,
    },
    trophyName: {
      type: String as PropType<string | undefined>,
      default: undefined,
    },
    lastRefreshedPopulation: {
      type: Number,
      required: true,
    },
    lastRefreshedTimestamp: {
      type: Number,
      required: true,
    },
    targetPopulation: {
      type: Number,
      required: true,
    },
    habSpace: {
      type: Number,
      required: true,
    },
    offlineIHR: {
      type: Number,
      required: true,
    },
  },
  setup(props) {
    const {
      trophyLevel,
      trophyName,
      lastRefreshedPopulation,
      lastRefreshedTimestamp,
      targetPopulation,
      offlineIHR,
    } = toRefs(props);
    const trophyDisplayName = computed(() =>
      trophyName.value !== undefined
        ? trophyName.value
        : `Enlightenment ${trophyLevel.value} Trophy`
    );
    const completionForecast = computed(() => {
      if (lastRefreshedPopulation.value < targetPopulation.value && offlineIHR.value > 0) {
        const timeToCompleteSeconds =
          ((targetPopulation.value - lastRefreshedPopulation.value) / offlineIHR.value) * 60;
        return dayjs(lastRefreshedTimestamp.value + timeToCompleteSeconds * 1000);
      } else {
        return null;
      }
    });
    const now = ref(dayjs());
    const completionForecastDays = computed(() =>
      completionForecast.value !== null
        ? completionForecast.value.diff(now.value, 'day', true)
        : null
    );
    const completionForecastFormattedDuration = computed(() =>
      completionForecast.value !== null
        ? formatDuration(completionForecast.value.diff(now.value, 'second', true), true)
        : null
    );
    const refreshIntervalId = setInterval(() => {
      now.value = dayjs();
    }, 30000);
    onBeforeUnmount(() => {
      clearInterval(refreshIntervalId);
    });
    return {
      trophyDisplayName,
      completionForecast,
      completionForecastDays,
      completionForecastFormattedDuration,
    };
  },
});
</script>
