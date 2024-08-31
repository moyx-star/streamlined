<script setup lang="ts">
import { useFieldArray, useForm } from 'vee-validate'
import { watch } from 'vue'

import type { QuoteBlockInfo } from '@/types/block'

// 怎么传入 props，在这里如何定义？
const props = defineProps<{
  blockInfo: QuoteBlockInfo
}>()
console.log('🚀 ~ file: QuoteSetting.vue:10 ~ blockInfo:', props.blockInfo)

const emit = defineEmits<{ (event: 'change', block: QuoteBlockInfo): void }>()

const { values, defineInputBinds } = useForm({
  initialValues: {
    content: props.blockInfo.props.content
  }
})
const { fields, push } = useFieldArray('blocks')

const content = defineInputBinds('content')

watch([values], ([newValues]) => {
  console.log(
    '🚀 ~ file: QuoteSetting.vue:26 ~ watch ~ { ...props.blockInfo, props: { ...props.blockInfo.props, ...newValues } }:',
    { ...props.blockInfo, props: { ...props.blockInfo.props, ...newValues } }
  )
  emit('change', { ...props.blockInfo, props: { ...props.blockInfo.props, ...newValues } })
})


</script>

<template>
  <div class="quote-setting">
    <div>
      {{ props.blockInfo.type }}
    </div>
    <input class="content-input" v-bind="content" />
    <input v-for="field in fields" :key="field.key" class="content-input" v-model="field.value" />
    <button class="add-button" @click="push(new Date().toLocaleTimeString())">添加</button>

    <vue-json-pretty showIcon showLineNumber editable :data="blockInfo" />
  </div>
</template>

<style scoped>
.quote-setting {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
  font-size: var(--font-size-large);
  border-radius: 8px;
}
.content-input {
  width: 100%;
  height: 32px;
  margin-top: 8px;
  padding: 0 8px;
  border: 1px solid var(--color-gray-300);
  border-radius: 8px;
  outline-style: none;
  color: var(--color-gray-800);
}

.add-button {
  margin-top: 6px;
  padding: 4px 12px;
  border-radius: 8px;
  background-color: var(--color-white);
  border: 1px solid var(--color-gray-300);
  cursor: pointer;
}
</style>
