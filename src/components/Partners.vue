<template>
    <section ref="sectionRef" class="py-20 px-[10%] w-full bg-white">
        <div class="max-w-7xl mx-auto">
            <!-- Section Header -->
            <div class="max-w-7xl mx-auto text-start mb-12">
                <h2 class="text-4xl md:text-5xl font-bold mb-4">
                    Our partners
                </h2>
                <p class="text-4xl md:text-4xl">
                    are take care of our fund and help us with many questions
                </p>
            </div>

            <!-- Partners Grid -->
            <div
                class="grid grid-cols-2 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8"
            >
                <div
                    v-for="(partner, index) in partners"
                    :key="index"
                    class="partner-card"
                    ref="partnerRefs"
                >
                    <div
                        class="partner-card-inner "
                    >
                        <img
                            :src="partner.image_partner"
                            :alt="'Partner logo'"
                            class="max-h-16 align-middle grayscale"
                        />
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

// Register GSAP plugin
gsap.registerPlugin(ScrollTrigger);

// Partners data
const partners = [
    {
        image_partner: new URL(
            "@/assets/images/62aa3401042d425391235aaa_Logo-adviRES.jpg",
            import.meta.url
        ).href,
    },
    {
        image_partner: new URL(
            "@/assets/images/62aa33e1380de680b7110ebf_ovid-wind-logo.png",
            import.meta.url
        ).href,
    },
    {
        image_partner: new URL(
            "@/assets/images/6265d803f32677c8352bc485_tbs_logo1-02.png",
            import.meta.url
        ).href,
    },
    {
        image_partner: new URL(
            "@/assets/images/6265d7edf0c46ce38f559328_download.png",
            import.meta.url
        ).href,
    },
    {
        image_partner: new URL(
            "@/assets/images/6265d7d930a5c2377aedf9d4_logo_laudis_accountancy.png",
            import.meta.url
        ).href,
    },
    {
        image_partner: new URL(
            "@/assets/images/6265d7c51ad96b1cbf31b0c0_photo_2022-04-21 20.43.09.jpeg",
            import.meta.url
        ).href,
    },
];

// Refs to elements
const partnerRefs = ref<HTMLElement[]>([]);
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
    // Partners animation
    const items = partnerRefs.value;
    const rowSize =
        window.innerWidth < 768
            ? 2 // mobile: 2 kolom per baris
            : 4; // desktop: 3 kolom per baris
    const rows: HTMLElement[][] = [];

    // Group into rows
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
            // markers: true // For debugging
        });
    });
};

onMounted(async () => {
    await nextTick();
    setupAnimations();
});

onBeforeUnmount(() => {
    ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
});
</script>

<style scoped>
.partner-card {
    overflow: visible;
    position: relative;
}

.partner-card-inner {
    transition: transform 0.3s ease, box-shadow 0.3s ease,
        border-color 0.3s ease;
    will-change: transform;
    height: 100%;
    min-height: 180px;
}

.partner-card:hover .partner-card-inner {
    transform: translate(0.5em, -0.5em);
    border-color: #3e88ff;
    z-index: 1;
}

.partner-card h3 {
    font-weight: 800;
    letter-spacing: -0.5px;
}

.partner-card p {
    font-weight: 300;
    letter-spacing: 0.5px;
}

</style>
