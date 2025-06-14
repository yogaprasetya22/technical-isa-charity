<template>
    <section ref="sectionRef" class="p-[10%] w-full bg-[#ff97d0]">
        <div class="max-w-7xl mx-auto">
            <div class="text-start mb-16">
                <h2 class="text-4xl md:text-5xl font-bold mb-4">
                    Socially active citizens
                </h2>
                <p class="text-4xl md:text-4xl">support us</p>
            </div>

            <!-- Supporters Grid -->
            <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8">
                <div
                    v-for="(supporter, index) in supporters"
                    :key="index"
                    class="supporter-card"
                    ref="supporterRefs"
                >
                    <div class="supporter-card-inner ">
                        <img
                            :src="supporter.logo"
                            :alt="supporter.team_name"
                            class="w-full align-middle grayscale"
                        />
                        <div class="mt-2 space-y-2 text-center p-4">
                            <p class="text-xl text-gray-900">
                                {{ supporter.team_name }}
                            </p>
                            <p class="text-md text-gray-600">
                                {{ supporter.team_title }}
                            </p>
                        </div>
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

gsap.registerPlugin(ScrollTrigger);

const supporterRefs = ref<HTMLElement[]>([]);
const sectionRef = ref<HTMLElement | null>(null);

const supporters = [
    {
        team_name: "Oleksiy Tritenko",
        team_title: "theater and film actor",
        logo: new URL(
            "@/assets/images/628398012a964488bb09078d_13.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Stanislav Boklan",
        team_title: "People's Artist of Ukraine",
        logo: new URL(
            "@/assets/images/628397b3f06d2066cfe8e9a2_14.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Alexander Rudinsky",
        team_title: "theater and film actor",
        logo: new URL(
            "@/assets/images/627fcdfcb2137dcf7db53e7f_12.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Oksana Zhdanova",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fcdd72d729d8ecacf6de7_11.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Natalka Denisenko",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fcdaedc0f8eea17c92429_10.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Maksym Samchyk",
        team_title: "theater and film actor",
        logo: new URL(
            "@/assets/images/627fcd87d51b9051dfcbf349_09.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Kateryna Vyshneva",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fcd5ca9508410b3a0d824_08.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Irma Vitovska",
        team_title: "Honored Actress of Ukraine",
        logo: new URL(
            "@/assets/images/627fcd3288758547e6ee0e22_07.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Igor Portyanko",
        team_title: "actor of theater and cinema",
        logo: new URL(
            "@/assets/images/627fcd1790c298c49bc7c436_06.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Dmytro Surzhikov",
        team_title: "Honored Artist of Ukraine",
        logo: new URL(
            "@/assets/images/627fccf0fc672a5dcfa87ee9_05.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Daria Barihashvili",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fccc57c3cb5177369015e_04.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Vera Kobzar",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fcc9bc50d7acc036982a9_03.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "Andriy Fedynchyk",
        team_title: "",
        logo: new URL(
            "@/assets/images/627fcc7339e6f5389ed03013_02.jpg",
            import.meta.url
        ).href,
    },
    {
        team_name: "theater and film actor",
        team_title: "theater and film actress",
        logo: new URL(
            "@/assets/images/627fcc4b694dfd4d65a13230_01.jpg",
            import.meta.url
        ).href,
    },
];

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
            : 4; // desktop: 3 kolom per baris
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
            // markers: true // Uncomment untuk debugging
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
    overflow: visible;
    position: relative;
}

.supporter-card-inner {
    transition: transform 0.3s ease;
    will-change: transform;
    height: 100%;
}

.supporter-card:hover .supporter-card-inner {
    transform: translate(0.5em, -0.5em);
    z-index: 1;
}

.supporter-card img {
    transition: transform 0.3s ease;
}

.supporter-card:hover img {
    transform: scale(1.05);
}
</style>