<template>
  <div class="p-[10%] w-full bg-[#3e88ff]">
    <div class="max-w-7xl mx-auto">
      <!-- Section Header -->
      <div class="text-start mb-16">
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">Our projects</h2>
        <p class="text-4xl md:text-4xl text-white">
          are very different in terms of priority, scale and complexity of implementation
        </p>
      </div>

      <!-- Projects Grid -->
      <div class="container space-y-2">
        <div
          v-for="(project, index) in projects"
          :key="index"
          class="project-card bg-gray-50 rounded-xl p-8 shadow-sm hover:shadow-md transition-all"
          ref="projectRefs"
        >
          <div class="text-indigo-600 text-3xl mb-4">{{ project.number }}</div>
          <h3 class="text-2xl font-bold text-gray-900 mb-3">{{ project.title }}</h3>
          <p class="text-gray-600">{{ project.description }}</p>
          <div class="mt-6 h-1 w-16 bg-indigo-500 rounded-full"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

// Register GSAP plugin
gsap.registerPlugin(ScrollTrigger)

// Project data
const projects = [
  {
    id: 1,
    number: '01',
    title: 'Emergency Aid. WAR 2022.',
    description: 'Providing food and medicine to the shelters and animals which lost their homes and families due to the war'
  },
  {
    id: 2,
    number: '02',
    title: 'Non-commercial feed line',
    description: 'Construction of industrial production base where food for shelters will be produced on a free basis'
  },
  {
    id: 3,
    number: '03',
    title: 'Education and Control',
    description: 'Lectures on communication, organisation and coordination of processes, control over the use of aid'
  }
]

// Refs to project elements
const projectRefs = ref<HTMLElement[]>([])

// Scroll-triggered animation
const setupAnimations = () => {
  projectRefs.value.forEach((el, index) => {
    if (!el) return

    // Set initial state
    gsap.set(el, {
      x: -100,
      opacity: 0,
      scale: 0.95
    })

    ScrollTrigger.create({
      trigger: el,
      start: "top 80%",
      end: "bottom 20%",
      onEnter: () => animateCardIn(el, index),
      onEnterBack: () => animateCardIn(el, index),
      onLeave: () => animateCardOut(el),
      onLeaveBack: () => animateCardOut(el),
      // markers: true // aktifkan kalau mau debug scroll position
    })
  })

  const animateCardIn = (element: HTMLElement, delayIndex: number) => {
    gsap.to(element, {
      x: 0,
      opacity: 1,
      scale: 1,
      duration: 0.8,
      delay: delayIndex * 0.1,
      ease: 'back.out(1.5)'
    })
  }

  const animateCardOut = (element: HTMLElement) => {
    gsap.to(element, {
      x: -100,
      opacity: 0,
      scale: 0.95,
      duration: 0.5,
      ease: 'power3.in'
    })
  }
}

onMounted(async () => {
  await nextTick()
  setupAnimations()
})

onBeforeUnmount(() => {
  ScrollTrigger.getAll().forEach(trigger => trigger.kill())
})
</script>

<style scoped>
.project-card {
  will-change: transform, opacity;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.project-card:hover {
  transform: translateY(-5px) scale(1.02);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
}

.project-card div:last-child {
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.project-card:hover div:last-child {
  transform: scaleX(1);
}

.project-card * {
  will-change: transform, opacity;
}
</style>
