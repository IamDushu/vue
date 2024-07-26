<script setup>
import BookingItem from '@/components/BookingItem.vue'
import { onMounted } from 'vue'
import LoadingBookingItem from '@/components/LoadingBookingItem.vue'
import EventList from '@/components/EventList.vue'
import useBookings from '@/composables/useBookings.js'

const {bookings, loading, cancelBooking, fetchBookings} = useBookings()

onMounted(() => {
  fetchBookings()
})
</script>

<template>
  <main class="container px-4 mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking App</h1>
    <section class="flex flex-col lg:flex-row gap-4">
      <EventList />
      <div class="space-y-8 lg:w-1/3 border-dashed border-[2px] p-4 rounded-md">
        <h2 class="text-2xl font-medium">Your Bookings</h2>
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
      </div>
    </section>
  </main>
</template>
