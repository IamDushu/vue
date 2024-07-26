<script setup>
import BookingItem from '@/components/BookingItem.vue'
import { ref, onMounted } from 'vue'
import LoadingBookingItem from '@/components/LoadingBookingItem.vue'
import EventList from '@/components/EventList.vue'

const bookings = ref([])
const bookingsLoading = ref(false)

const findBookingById = (id) => {
  return bookings.value.findIndex(booking => booking.id === id)
}

const fetchBookings = async () => {
  bookingsLoading.value = true
  try {
    const response = await fetch('http://localhost:3001/bookings')
    bookings.value = await response.json()
  } finally {
    bookingsLoading.value = false
  }
}

const handleRegistration = async (event) => {
  if(bookings.value.some(booking => booking.eventId === event.id && booking.userId === 1)) {
    alert('You have already registered for this event!')
    return
  }

  const newBooking = {
    id: Date.now().toString(),
    userId: 1,
    eventId: event.id,
    eventTitle: event.title,
    status: 'pending'
  }

  bookings.value.push(newBooking)

  try {
    const response = await fetch('http://localhost:3001/bookings', {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify({...newBooking, status: 'confirmed'}),
    })

      if (response.ok) {
        const index = findBookingById(newBooking.id)
        bookings.value[index] = await response.json()
      } else {
        throw new Error('Failed to confirm booking')
      }
    } catch (error) {
      console.error(`Failed to confirm booking: `, error)
      bookings.value = bookings.value.filter(booking => booking.id !== newBooking.id)
  }

}

const cancelBooking = async (bookingId) => {
  const index = findBookingById(bookingId)
  const originalBooking = bookings.value[index]
  bookings.value.splice(index, 1)

  try {
    const response = await fetch(`http://localhost:3001/bookings/${bookingId}`, {
      method: 'DELETE',
    })

    if (!response.ok) {
      throw new Error('Booking could not be cancelled.')
    }
  } catch (e) {
    console.log('Failed to delete booking: ', e)
    bookings.value.splice(index, 0, originalBooking)
  }
}

onMounted(() => {
  fetchBookings()
})
</script>

<template>
  <main class="container px-4 mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking App</h1>
    <section class="flex flex-col lg:flex-row gap-4">
      <EventList @register="handleRegistration($event)" />
      <div class="space-y-8 lg:w-1/3 border-dashed border-[2px] p-4 rounded-md">
        <h2 class="text-2xl font-medium">Your Bookings</h2>
        <section class="grid grid-cols-1 gap-4">
          <template v-if="!bookingsLoading && bookings.length > 0">
            <BookingItem v-for="booking in bookings"
                         :title="booking.eventTitle" :status="booking.status" :key="booking.id"
                         @cancelled="cancelBooking(booking.id)"
            />
          </template>
          <template v-else-if="!bookingsLoading">
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
