<script setup>
import EventList from '@/components/EventList.vue'
import BookingList from '@/components/BookingList.vue'
</script>

<template>
  <main class="container px-4 mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking App</h1>
    <section class="flex flex-col lg:flex-row gap-4">
      <EventList />
      <div class="space-y-8 lg:w-1/3 border-dashed border-[2px] p-4 rounded-md">
        <h2 class="text-2xl font-medium">Your Bookings</h2>
        <BookingList />
      </div>
    </section>
  </main>
</template>
