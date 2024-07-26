<script setup>
import RoundButton from '@/components/RoundButton.vue'
import SectionCard from '@/components/SectionCard.vue'
import { computed } from 'vue'
import { BadgeCheck, LoaderCircle } from 'lucide-vue-next'

const props = defineProps({
  title: {
    type: String,
  },
  status: String
})

defineEmits(['cancelled'])

const pending = computed(() => props.status === 'pending')
const icon = computed(() => pending.value ? LoaderCircle : BadgeCheck)

</script>

<template>
  <SectionCard>
    <div class="flex justify-between">
      <div class="flex items-center space-x-2">
        <div>{{title}}</div>
        <div>
          <component :is="icon" :class="{'animate-spin text-orange-500': pending, 'text-green-500': true}" />
        </div>
      </div>
      <RoundButton variant="danger" @click="$emit('cancelled')">Cancel</RoundButton>
    </div>
  </SectionCard>
</template>

