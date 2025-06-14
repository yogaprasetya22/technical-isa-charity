<template>
    <section ref="sectionRef" class="p-[10%] w-full bg-white text-black">
        <div class="max-w-7xl mx-auto text-start mb-12">
            <h2 class="text-4xl md:text-5xl font-bold mb-4">Our team</h2>
            <p class="text-4xl md:text-4xl">
                consists of completely different people who are united by a
                common desire – to help
            </p>
        </div>

        <div
            class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-x-10 gap-4 max-w-7xl mx-auto"
        >
            <div
                v-for="(member, index) in team"
                :key="index"
                class="team-card"
                ref="teamRefs"
            >
                <div
                    class="team-card-inner bg-[#fcf944] overflow-hidden text-center border-2 border-black"
                >
                    <div v-if="member.image" class="w-full aspect-square p-5">
                        <img
                            :src="member.image"
                            alt="Team member photo"
                            class="w-full h-full align-middle grayscale"
                        />
                    </div>
                    <div
                        v-else
                        class="w-full aspect-square flex items-center justify-center bg-white"
                    >
                        <img
                            :src="member.placeholder"
                            alt="Vacancy icon"
                            class="w-1/2 h-1/2 object-contain"
                        />
                    </div>

                    <div class="p-4 space-y-1">
                        <h3 class="font-semibold text-sm md:text-lg">{{ member.name }}</h3>
                        <p class="text-sm md:text-sm text-gray-700">{{ member.role }}</p>
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

const team = [
    {
        name: "Daryna Deriy",
        role: "Chairman of the Board",
        image: new URL(
            "@/assets/images/62744ee9fce7fbaf400c22f8_2.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Polina Snisarenko–Kulchytska",
        role: "Curator of the Foundation",
        image: new URL(
            "@/assets/images/62744edaac91b4183ac27e10_5.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Bohdan Kulchytsky",
        role: "Executive Manager",
        image: new URL(
            "@/assets/images/62744ec97586a996f48f7a85_1.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Mykhaylo Deriy",
        role: "Operation Manager",
        image: new URL(
            "@/assets/images/62744eae22e10d8a15f909ef_9.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Ksenia Nikishina",
        role: "Office manager",
        image: new URL(
            "@/assets/images/62744e8e05f6c22bac85252a_7.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Anna Andriychuk",
        role: "SMM-manager",
        image: new URL(
            "@/assets/images/62744f0eb7cd3462ef475ebf_6.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Anastasia Kovalchuk",
        role: "Director",
        image: new URL(
            "@/assets/images/62744e6761e425c6956af3a5_3.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Basil Gloo",
        role: "Web Developer",
        image: new URL(
            "@/assets/images/626ae0bcd92bf48017c14083_basil-square.png",
            import.meta.url
        ).href,
        placeholder: "@/assets/images/placeholder.png",
    },
    {
        name: "Lina Yakobchuk",
        role: "Designer",
        image: new URL(
            "@/assets/images/626ae01da2a805d176b1606b_Якобчук.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Anastasia Yevchenko",
        role: "Translator",
        image: new URL(
            "@/assets/images/627fc796cf3602b53a7512cf_4.jpg",
            import.meta.url
        ).href,
    },
    {
        name: "Office Manager",
        role: "open vacancy",
        image: new URL(
            "@/assets/images/627fc7fcdc0f8e9cbcc8ffed_isa-logo-dog-clean.svg",
            import.meta.url
        ).href,
        placeholder: "@/assets/images/",
    },
    {
        name: "SEO specialist",
        role: "open vacancy",
        image: new URL(
            "@/assets/images/627fc81139e6f5dca2d02054_isa-logo-cat-clean.svg",
            import.meta.url
        ).href,
        placeholder: "@/assets/images/",
    },
];

const teamRefs = ref<HTMLElement[]>([]);
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
    const items = teamRefs.value;
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
    gsap.registerPlugin(ScrollTrigger);
    await nextTick();
    setupAnimations();
});

onBeforeUnmount(() => {
    ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
});
</script>

<style scoped>
.team-card {
    overflow: visible;
    position: relative;
}

.team-card-inner {
    transition: transform 0.3s ease;
    will-change: transform;
    height: 100%;
}

.team-card:hover .team-card-inner {
    transform: translate(0.5em, -0.5em);
    z-index: 1;
    box-shadow: 8px 8px 20px rgba(0, 0, 0, 0.1);
}

.team-card img {
    transition: transform 0.3s ease;
}

.team-card:hover img {
    transform: scale(1.05);
}
</style>
