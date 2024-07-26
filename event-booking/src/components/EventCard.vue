<script setup>
import RoundButton from '@/components/RoundButton.vue'
import SectionCard from '@/components/SectionCard.vue'

defineProps({
  title: String,
  when: String,
  description: String
})

defineEmits(['register'])
</script>

<template>
  <SectionCard>
    <template #header>
      {{ title }}
    </template>
    <p>{{ when }}</p>
    <template #footer>
      <div class="space-y-4">
        <p class="text-xs font-medium">{{ description }}</p>
        <div class="flex justify-end">
          <RoundButton @click="$emit('register')" >Register</RoundButton>
        </div>
      </div>
    </template>
  </SectionCard>
</template>
