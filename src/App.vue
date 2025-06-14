<template>
  <section ref="section" class="min-h-screen flex items-center justify-center bg-white">
    <div class="text-center opacity-0" ref="content">
      <h2 class="text-4xl font-bold mb-4">Tentang Kami</h2>
      <p class="text-gray-600">Kami adalah organisasi nirlaba...</p>
    </div>
  </section>
</template>

<script>
import { onMounted, ref } from 'vue'
import gsap from 'gsap'

export default {
  name: 'Section1',
  setup() {
    const section = ref(null)
    const content = ref(null)

    onMounted(() => {
      gsap.to(content.value, {
        opacity: 1,
        y: -20,
        duration: 1,
        ease: 'power2.out'
      })
    })

    return {
      section,
      content
    }
  }
}
</script>

<style lang="scss" scoped>
/* Gunakan Tailwind untuk sebagian besar styling, SCSS untuk kustom tambahan jika perlu */
</style>
