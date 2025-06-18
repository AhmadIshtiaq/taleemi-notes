<template>
  <div class="container mx-auto px-4 py-8">
    <!-- Subject Header -->
    <div class="mb-8 text-center">
      <UBreadcrumb 
        :links="breadcrumbs" 
        class="justify-center mb-4" 
      />
      <h1 class="text-3xl font-bold">
        {{ capitalize(grade) }} Class {{ subject }} Notes
      </h1>
      <p class="text-gray-600 mt-2">
        Chapter-wise notes, past papers, and MCQs
      </p>
    </div>

    <!-- Chapter Filter Tabs -->
    <UTabs 
      :items="chapters" 
      v-model="selectedChapter"
      class="mb-8"
    >
      <template #item="{ item }">
        <span class="flex items-center gap-1">
          <UIcon name="i-heroicons-document-text" class="h-4 w-4" />
          Chapter {{ item.label }}
        </span>
      </template>
    </UTabs>

    <!-- Notes Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <NoteCard
        v-for="note in filteredNotes"
        :key="note._id"
        :title="note.title"
        :subject="subject"
        :grade="grade"
        :chapter="note.chapter"
        :description="note.description"
        :slug="note.slug"
      />
    </div>

    <!-- Past Papers Section -->
    <div class="mt-12">
      <h2 class="text-2xl font-semibold mb-4 flex items-center gap-2">
        <UIcon name="i-heroicons-archive-box" class="h-6 w-6 text-primary-500" />
        Past Papers
      </h2>
      <div class="space-y-4">
        <!-- <UPdfCard
          v-for="paper in pastPapers"
          :key="paper.year"
          :year="paper.year"
          :board="paper.board"
          :url="paper.url"
        /> -->
      </div>
    </div>
  </div>
</template>

<script setup>
// Get route parameters
const route = useRoute()
const grade = route.params.grade.replace('-', ' ') // '9th' or '1st-year' → '1st year'
const subject = capitalize(route.params.subject) // 'physics' → 'Physics'

// Fetch notes from content/notes directory
const { data: notes } = await useAsyncData(`${grade}-${subject}-notes`, () => 
  queryContent('notes')
    .where({ 
      grade: { $eq: grade },
      subject: { $eq: subject.toLowerCase() } 
    })
    .sort({ chapter: 1 })
    .find()
)

// Generate chapter tabs from available notes
const chapters = computed(() => {
  return notes.value.map(note => ({
    label: note.chapter.toString(),
    value: note.chapter
  }))
})

// Filter notes by selected chapter
const selectedChapter = ref(1)
const filteredNotes = computed(() => {
  return notes.value.filter(note => note.chapter === selectedChapter.value)
})

// Mock past papers data (replace with real data)
// const pastPapers = [
//   { year: 2024, board: 'BISE Lahore', url: '/pdfs/past-papers/2024-lahore.pdf' },
//   { year: 2023, board: 'BISE Lahore', url: '/pdfs/past-papers/2023-lahore.pdf' },
//   { year: 2022, board: 'BISE Gujranwala', url: '/pdfs/past-papers/2022-gujranwala.pdf' }
// ]

// Breadcrumb navigation
const breadcrumbs = [
  { label: 'Home', icon: 'i-heroicons-home', to: '/' },
  { label: `${capitalize(grade)} Class`, to: `/class/${grade.toLowerCase().replace(' ', '-')}` },
  { label: subject, to: null }
]

// Helper function to capitalize strings
const capitalize = (str) => {
  return str.split('-').map(word => 
    word.charAt(0).toUpperCase() + word.slice(1)
  ).join(' ')
}
</script>

<style scoped>
/* Custom tab active state */
:deep(.tab-active) {
  @apply border-primary-500 text-primary-600 dark:text-primary-400;
}
</style>