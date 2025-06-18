<template>
  <div class="space-y-16">
    <!-- Hero Section -->
    <section class="text-center py-12">
      <h1 class="text-4xl font-bold mb-4 text-primary">Welcome to TaleemiNotes.pk</h1>
      <p class="text-gray-600 mb-6">Free notes, past papers & quizzes for Matric & Intermediate students in Pakistan.</p>
      <UInput
        v-model="search"
        placeholder="Search notes e.g. 9th class physics"
        icon="i-heroicons-magnifying-glass-20-solid"
        size="lg"
        class="max-w-xl mx-auto"
      />
    </section>

    <!-- Classes Section -->
    <section>
      <h2 class="text-2xl font-semibold text-center mb-6">Select Your Class</h2>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-6 max-w-6xl mx-auto">
        <UCard v-for="item in classes" :key="item.name" class="hover:shadow-lg">
          <template #header>
            <h3 class="text-lg font-bold">{{ item.name }}</h3>
          </template>
          <p>{{ item.description }}</p>
          <template #footer>
            <UButton :to="item.link" color="primary">View Notes</UButton>
          </template>
        </UCard>
      </div>
    </section>

    <!-- Featured Notes Section -->
    <section>
      <h2 class="text-2xl font-semibold text-center mb-6">Popular Subjects</h2>
      <div class="flex flex-wrap justify-center gap-4">
        <UBadge
          v-for="subject in subjects"
          :key="subject"
          variant="subtle"
          class="cursor-pointer"
        >
          {{ subject }}
        </UBadge>
      </div>
    </section>

    <!-- Past Papers Section -->
    <section class="text-center py-10 bg-gray-50 dark:bg-gray-900">
      <h2 class="text-xl font-semibold mb-3">Need Past Papers?</h2>
      <p class="mb-4">Download solved board exam papers for all subjects.</p>
      <UButton color="primary" to="/past-papers">Browse Past Papers</UButton>
    </section>
  </div>
</template>

<script setup>
const search = ref('')
const classes = [
  { name: '9th Class', description: 'All subjects notes for class 9', link: '/class/9th' },
  { name: '10th Class', description: 'Matric part 2 notes', link: '/class/10th' },
  { name: '1st Year', description: 'FSc, ICS, FA part 1 notes', link: '/class/1st-year' },
  { name: '2nd Year', description: 'FSc, ICS, FA part 2 notes', link: '/class/2nd-year' },
]
const subjects = ['Physics', 'Chemistry', 'Math', 'Biology', 'Urdu', 'English', 'Computer']
</script>
