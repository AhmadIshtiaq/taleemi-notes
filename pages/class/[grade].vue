<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-bold mb-6">{{ $route.params.grade }} Notes</h1>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <NoteCard 
        v-for="subject in subjects" 
        :key="subject" 
        :subject="subject" 
      />
    </div>
  </div>
</template>

<script setup>
const subjects = ["Physics", "Math", "Chemistry", "Biology", "English", "Urdu"]
</script>