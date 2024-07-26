<script setup>
import { onMounted, ref } from 'vue'
import EventCard from '@/components/EventCard.vue'
import LoadingEventCard from '@/components/LoadingEventCard.vue'

defineEmits(['register'])

const events = ref([])
const loading = ref(false)

const fetchEvents = async () => {
 loading.value = true
  try {
    const response = await fetch('http://localhost:3001/events')
    events.value = await response.json()
  } finally {
   loading.value = false
  }
}

onMounted(() => {
  fetchEvents()
})
</script>

<template>
    <div class="space-y-8 lg:w-2/3">
      <h2 class="text-2xl font-medium">All Events</h2>
      <section class="grid grid-cols-2 lg:grid-cols-3 gap-8">
        <template v-if="!loading">
          <template v-if="events.length">
            <EventCard v-for="event in events" :key="event.id"
                       :title="event.title" :when="event.date" :description="event.description"
                       @register="$emit('register', event)"
            />
          </template>
          <template v-else>
            <div class="col-span-2 text-center text-gray-500 lg:col-span-3">
              No events yet.
            </div>
          </template>
        </template>
        <template v-else>
          <LoadingEventCard v-for="i in 4" :key="i" />
        </template>
      </section>
    </div>
</template>
