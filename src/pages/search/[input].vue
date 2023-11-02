<script setup lang="ts">
const props = defineProps<{ input: string }>()
const router = useRouter()
const history = searchHistory()
const { t } = useI18n()

watchEffect(() => {
  history.addInput(props.input)
})
</script>

<template>
  <div relative p-6>
    <div text-4xl>
      <div i-carbon-search-locate-mirror inline-block />
    </div>
    <template v-if="history.input.length">
      <div max-h-16 overflow-auto>
        History
        <ul flex flex-col-reverse text-sm>
          <li v-for="prevText in history.previousInputs" :key="prevText" text-coolGray-500>
            <RouterLink :to="`/search/${prevText}`" replace underline>
              {{ prevText }}
            </RouterLink>
          </li>
        </ul>
      </div>
    </template>
    <button
      m="3 t6" text-sm btn
      @click="router.back()"
    >
      {{ t('button.back') }}
    </button>
    <p mb-2 mt-2 text-4xl>
      {{ t('search.current', { search: props.input }) }}
    </p>

    <StarredReposVue :filter="props.input" />
  </div>
</template>
