<script setup>
import RoundButton from '@/components/RoundButton.vue'
import SectionCard from '@/components/SectionCard.vue'

defineProps({
  title: {
    type: String,
  },
  status: String
})

defineEmits(['cancelled'])
</script>

<template>
  <SectionCard>
    <div class="flex justify-between">
      <div class="flex items-center space-x-2">
        <div>{{title}}</div>
        <div>{{status}}</div>
      </div>
      <RoundButton variant="danger" @click="$emit('cancelled')">Cancel</RoundButton>
    </div>
  </SectionCard>
</template>

