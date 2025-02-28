<template>
  <slot>
    <div v-if="items?.length" :class="wrapperClass">
      <FormRadio
        class="w-full"
        v-for="(item, index) in items"
        :key="index"
        v-bind="{ disabled }"
        v-model="activeRadio"
        :label="item[labelKey]"
        :value="item[valueKey]"
        :name="radioName"
        :error="error"
        :class="itemClass"
        @click="activeRadio = !disabled ? item[valueKey] : modelValue"
      />
    </div>
  </slot>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

interface Props {
  modelValue: string | number | object
  items: Array<object>
  labelKey?: string
  error?: boolean
  valueKey?: string
  wrapperClass?: string
  disabled?: boolean
  itemClass?: string
}

const props = withDefaults(defineProps<Props>(), {
  wrapperClass: 'flex flex-wrap lg:flex-nowrap gap-4',
  labelKey: 'name',
  valueKey: 'id',
  disabled: false,
})

let activeRadio = ref(props.modelValue)

const emit = defineEmits<{
  (e: 'update:modelValue', value: string | number | object): void
}>()

const radioName = `k-radio-${Math.floor(Math.random() * 1000)}`

const value = ref<string | number | object>([])

watch(
  () => activeRadio.value,
  (newValue) => {
    if (newValue !== value.value) {
      value.value = newValue
    }
    if (!props.disabled) {
      emit('update:modelValue', value.value)
    }
  }
)
</script>
