<template>
    <div class="bg-[#fcf944] text-black py-24 px-4 overflow-hidden">
        <div class="max-w-6xl mx-auto text-center mb-12">
            <h2 class="text-4xl md:text-5xl font-bold mb-4">We want to help</h2>
            <p class="text-4xl md:text-4xl">
                homeless packs, volunteers, guardians, overstayers, shelters and
                other organisations
            </p>
        </div>

        <div
            ref="sectionRef"
            class="grid grid-cols-2 md:grid-cols-3 gap-4 max-w-6xl mx-auto"
        >
            <div
                class="supporter-card bg-white"
                v-for="(supporter, _) in supporters"
                :key="supporter"
                ref="supporterRefs"
            >
                <div class="supporter-card-inner border border-black z-10">
                    <div class="content text-center p-4 font-medium">
                        {{ supporter }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

// List supporter
const supporters = [
    "Shelter of Victoria Bulbyna",
    "City of faithful hearts",
    "Shelter of Natalia Tymoshenko",
    "Shelter of Jeanne Sass",
    "Shelter of Victoria Bondarenko",
    "Sterilization center",
    "Zoo chance",
    "Save lives",
    "City Center for the Treatment of Animals",
    "Society for the Protection of Animals in Sumy",
    "The right to life",
    "4 paws",
    "Animal Network",
    "Pet Rescue",
    "Forever Home",
    "Guardian Shelter",
    "New Chance",
    "Dog Heaven",
];

// Refs
const supporterRefs = ref<HTMLElement[]>([]);
const sectionRef = ref<HTMLElement | null>(null);

const animateRowIn = (row: HTMLElement[], delay: number) => {
    gsap.to(row, {
        opacity: 1,
        y: 0,
        duration: 0.6,
        stagger: 0.1,
        delay,
        ease: "back.out(1.7)",
        onComplete: () => {
            row.forEach((el) => gsap.set(el, { clearProps: "all" }));
        },
        onStart: () => {
            row.forEach((el) => gsap.set(el, { opacity: 0, y: 40 }));
        },
    });
};

const animateRowOut = (row: HTMLElement[]) => {
    gsap.to(row, {
        opacity: 0,
        y: 40,
        duration: 0.4,
        stagger: 0.05,
        ease: "power2.in",
    });
};

const setupAnimations = () => {
    const items = supporterRefs.value;
    const rowSize =
        window.innerWidth < 768
            ? 2 // mobile: 2 kolom per baris
            : 3; // desktop: 3 kolom per baris
    const rows: HTMLElement[][] = [];

    // Kelompokkan ke dalam baris
    for (let i = 0; i < items.length; i += rowSize) {
        rows.push(items.slice(i, i + rowSize));
    }

    rows.forEach((row, rowIndex) => {
        row.forEach((el) => gsap.set(el, { opacity: 0, y: 40 }));

        ScrollTrigger.create({
            trigger: row[0],
            start: "top 90%",
            end: "bottom 10%",
            onEnter: () => animateRowIn(row, rowIndex * 0.05),
            onEnterBack: () => animateRowIn(row, rowIndex * 0.05),
            onLeave: () => animateRowOut(row),
            onLeaveBack: () => animateRowOut(row),
            markers: true,
        });
    });
};

onMounted(async () => {
    await nextTick();
    setupAnimations();
});

onBeforeUnmount(() => {
    ScrollTrigger.getAll().forEach((t) => t.kill());
});
</script>

<style scoped>
.supporter-card {
    overflow: visible; /* penting agar border bisa “keluar” */
    position: relative;
}

.supporter-card-inner {
    transition: transform 0.3s ease;
    will-change: transform;
}

.supporter-card:hover .supporter-card-inner {
    transform: translate(0.5em, -0.5em);
    z-index: 1;
    box-shadow: 8px 8px 20px rgba(0, 0, 0, 0.1);
}
</style>
