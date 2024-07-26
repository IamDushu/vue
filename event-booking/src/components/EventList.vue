<script setup>
import { onMounted, ref } from 'vue'
import EventCard from '@/components/EventCard.vue'
import LoadingEventCard from '@/components/LoadingEventCard.vue'
import SectionCard from '@/components/SectionCard.vue'
import RoundButton from '@/components/RoundButton.vue'
import useBookings from '@/composables/useBookings.js'

const {handleRegistration} = useBookings()

const events = ref([])
const loading = ref(false)
const error = ref(null)

const fetchEvents = async () => {
  loading.value = true
  error.value = null;
  try {
    const response = await fetch('http://localhost:3001/events')
    events.value = await response.json()
  } catch (e) {
    error.value = e
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
      <template v-if="error">
        <SectionCard>
          <div class="space-y-4 items-center flex flex-col">
            <div class="text-red-500">
              Could not load events at the moment. Please try again.
            </div>
            <RoundButton @click="fetchEvents">Retry now</RoundButton>
          </div>
        </SectionCard>
      </template>
      <template v-else>
        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <template v-if="!loading">
            <template v-if="events.length">
              <EventCard v-for="event in events" :key="event.id"
                         :title="event.title" :when="event.date" :description="event.description"
                         @register="handleRegistration(event)"
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
      </template>
    </div>
</template>
