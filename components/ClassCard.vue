<template>
  <UCard :ui="{ base: 'h-full' }">
    <h3 class="text-xl font-bold">{{ grade }} Notes</h3>
    <p class="mt-2 text-gray-600">All subjects, past papers & quizzes</p>
    <template #footer>
      <UButton
        color="primary"
        :to="`/class/${grade.toLowerCase().replace(' ', '-')}`"
        label="View Notes"
      />
    </template>
  </UCard>
</template>

<script setup>
defineProps({
  grade: String
})
</script>