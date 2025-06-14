<template>
    <section class="py-16 px-[10%] w-full bg-[#31be32] text-black">
        <div class="w-full flex flex-row items-center justify-center">
            <!-- Contact Information -->
            <div class="text-start w-full">
                <h2 class="text-4xl md:text-6xl font-extrabold mb-6">
                    Contact us
                </h2>
                <div class="space-y-3 text-xl md:text-3xl">
                    <p>Kyiv, Ukraine</p>
                    <p>
                        <a
                            href="mailto:mail@isa.co.ua"
                            class="hover:underline transition-all duration-200"
                        >
                            mail@isa.co.ua
                        </a>
                    </p>
                </div>
                <div
                    class="flex items-center justify-start gap-5 text-2xl mt-6"
                >
                    <a href="#" aria-label="YouTube"
                        ><font-awesome-icon :icon="['fab', 'youtube']"
                    /></a>
                    <a href="#" aria-label="Instagram"
                        ><font-awesome-icon :icon="['fab', 'instagram']"
                    /></a>
                    <a href="#" aria-label="Facebook"
                        ><font-awesome-icon :icon="['fab', 'facebook']"
                    /></a>
                    <a href="#" aria-label="Patreon"
                        ><font-awesome-icon :icon="['fab', 'patreon']"
                    /></a>
                    <a href="#" aria-label="Telegram"
                        ><font-awesome-icon :icon="['fab', 'telegram']"
                    /></a>
                </div>
            </div>

            <!-- Logo -->
            <div ref="logoRef" class="mt-12">
                <img
                    src="@/assets/images/62647f9fbe07235ba9b28d67_star-white-with-animals.svg"
                    alt="ISA Logo"
                    class="w-5xl opacity-0"
                />
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

// Register GSAP plugin
gsap.registerPlugin(ScrollTrigger);

// Refs to elements
const logoRef = ref<HTMLElement | null>(null);

const setupAnimations = () => {
    // Logo animation
    if (logoRef.value) {
        gsap.to(logoRef.value.querySelector("img"), {
            opacity: 1,
            y: 0,
            duration: 1,
            ease: "elastic.out(1, 0.5)",
            scrollTrigger: {
                trigger: logoRef.value,
                start: "top 90%",
                toggleActions: "play none none reset",
            },
        });
    }
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
section {
    position: relative;
    overflow: hidden;
}

h2 {
    letter-spacing: -0.025em;
    line-height: 1.2;
}

a {
    display: inline-block;
    transition: transform 0.2s ease, text-decoration 0.2s ease;
}

a:hover {
    text-decoration: underline;
    transform: translateY(-1px);
}

/* Logo animation base state */
img {
    transform: translateY(20px);
}
</style>
