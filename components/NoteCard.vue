<template>
  <UCard 
    :ui="{
      base: 'h-full flex flex-col',
      body: { base: 'flex-1' },
      footer: { base: 'mt-auto' }
    }"
  >
    <!-- Header with subject icon -->
    <template #header>
      <div class="flex items-center gap-3">
        <UIcon 
          :name="subjectIcon" 
          class="w-8 h-8 text-primary-500" 
        />
        <h3 class="text-xl font-semibold">
          {{ subject }} - Chapter {{ chapter }}
        </h3>
      </div>
    </template>

    <!-- Content preview -->
    <p class="text-gray-600 line-clamp-3">
      {{ description || 'Comprehensive notes with solved examples...' }}
    </p>

    <!-- Footer with download/action buttons -->
    <template #footer>
      <div class="flex justify-between items-center">
        <UBadge color="green" variant="soft">
          {{ grade }}
        </UBadge>
        
        <UButton
          color="primary"
          :to="`/notes/${slug}`"
          label="View Notes"
          trailing-icon="i-heroicons-arrow-right-20-solid"
        />
      </div>
    </template>
  </UCard>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    default: 'Physics Notes'
  },
  subject: {
    type: String,
    required: true
  },
  grade: {
    type: String,
    default: '9th'
  },
  chapter: {
    type: [String, Number],
    default: 1
  },
  description: {
    type: String,
    default: ''
  },
  slug: {
    type: String,
    required: true
  }
})

// Dynamic icon based on subject
const subjectIcon = computed(() => {
  const icons = {
    Physics: 'i-heroicons-beaker',
    Math: 'i-heroicons-calculator',
    Chemistry: 'i-heroicons-flask',
    Biology: 'i-heroicons-leaf',
    English: 'i-heroicons-book-open',
    Urdu: 'i-heroicons-language'
  }
  return icons[props.subject] || 'i-heroicons-document-text'
})
</script>

<style scoped>
/* Custom hover effect */
.u-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease;
}
</style>