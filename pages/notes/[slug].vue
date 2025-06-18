<template>
  <div class="container mx-auto px-4 py-8 max-w-4xl">
    <!-- Back Button -->
    <UButton
      icon="i-heroicons-arrow-left-20-solid"
      label="Back to Chapters"
      color="gray"
      variant="ghost"
      class="mb-6"
      @click="goBack"
    />

    <!-- Note Header -->
    <div class="mb-8">
      <UBreadcrumb :links="breadcrumbs" class="mb-4" />
      <h1 class="text-3xl font-bold">{{ note.title }}</h1>
      <div class="flex gap-4 mt-2">
        <UBadge color="blue" variant="soft">
          {{ note?.grade }} Class
        </UBadge>
        <UBadge color="green" variant="soft">
          Chapter {{ note?.chapter }}
        </UBadge>
      </div>
    </div>

    <!-- Note Content -->
    <article class="prose dark:prose-invert max-w-none">
      <ContentRenderer :value="note">
        <template #empty>
          <p>No content found for this note.</p>
        </template>
      </ContentRenderer>
    </article>

    <!-- Download & Actions Section -->
    <div class="mt-8 border-t pt-6 flex flex-wrap gap-4 justify-between">
      <UButton
        color="primary"
        icon="i-heroicons-arrow-down-tray"
        label="Download PDF"
        :to="note.pdfUrl || `/pdfs/${note.slug}.pdf`"
        download
      />
      
      <div class="flex gap-2">
        <UButton
          color="gray"
          variant="ghost"
          icon="i-heroicons-printer"
          label="Print"
          @click="printNote"
        />
        <UButton
          color="gray"
          variant="ghost"
          icon="i-heroicons-share"
          label="Share"
          @click="shareNote"
        />
      </div>
    </div>

    <!-- Related Notes -->
    <div class="mt-12" v-if="relatedNotes.length > 0">
      <h2 class="text-2xl! font-semibold mb-4">Related Notes</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <NoteCard
          v-for="related in relatedNotes"
          :key="related._id"
          v-bind="related"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute()
const router = useRouter()

// Fetch the current note
const { data: note } = await useAsyncData(`note-${route.params.slug}`, () =>
  queryContent('notes')
    .where({ slug: { $eq: route.params.slug } })
    .findOne()
)

// Fetch related notes (same subject)
const { data: relatedNotes } = await useAsyncData(
  `related-${route.params.slug}`,
  () =>
    queryContent('notes')
      .where({
        subject: { $eq: note.value.subject },
        slug: { $ne: route.params.slug }
      })
      .limit(3)
      .find()
)

// Breadcrumbs navigation
const breadcrumbs = computed(() => [
  { label: 'Home', to: '/' },
  { label: `${note.value?.grade} Class`, to: `/class/${note.value?.grade?.toLowerCase().replace(' ', '-')}` },
  { label: note.value?.subject, to: `/class/${note.value?.grade?.toLowerCase().replace(' ', '-')}/${note.value.subject.toLowerCase()}` },
  { label: `Chapter ${note.value.chapter}`, to: null }
])

// Handle back button
const goBack = () => {
  router.push(`/class/${note.value?.grade?.toLowerCase().replace(' ', '-')}/${note.value.subject.toLowerCase()}`)
}

// Print function
const printNote = () => {
  window.print()
}

// Share function
const shareNote = async () => {
  try {
    await navigator.share({
      title: note.value.title,
      text: `Check out these ${note.value.subject} notes on TaleemiNotes.pk`,
      url: window.location.href
    })
  } catch (err) {
    // Fallback for browsers that don't support Web Share API
    const el = document.createElement('textarea')
    el.value = window.location.href
    document.body.appendChild(el)
    el.select()
    document.execCommand('copy')
    document.body.removeChild(el)
    useToast().add({ title: 'Link copied to clipboard!' })
  }
}
</script>

<style>
/* Prose styles for markdown content */
.prose {
  line-height: 1.6;
}
/* .prose h2 {
  @apply text-2xl font-bold mt-8 mb-4;
} */
/* .prose h3 {
  @apply text-xl font-semibold mt-6 mb-3;
} */
/* .prose p {
  @apply my-4;
}
.prose ul {
  @apply list-disc pl-6;
}
.prose img {
  @apply rounded-lg border my-4;
} */
</style>