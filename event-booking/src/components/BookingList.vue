<script setup>

import LoadingBookingItem from '@/components/LoadingBookingItem.vue'
import BookingItem from '@/components/BookingItem.vue'
import useBookings from '@/composables/useBookings.js'
import { onMounted } from 'vue'

const {bookings, loading, cancelBooking, fetchBookings} = useBookings()

onMounted(() => {
  fetchBookings()
})
</script>

<template>
  <section class="grid grid-cols-1 gap-4">
    <template v-if="!loading && bookings.length > 0">
      <BookingItem v-for="booking in bookings"
                   :title="booking.eventTitle" :status="booking.status" :key="booking.id"
                   @cancelled="cancelBooking(booking.id)"
      />
    </template>
    <template v-else-if="!loading">
      <p class="text-xs text-gray-900">No bookings to show. Register for an event.</p>
    </template>
    <template v-else>
      <LoadingBookingItem v-for="i in 2" :key="i" />
    </template>
  </section>
</template>
