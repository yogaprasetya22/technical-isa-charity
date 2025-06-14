<template>
    <section class="py-12 px-5 w-full bg-[#31be32] text-white md:py-16 md:px-[10%]">
        <div class="w-full flex flex-col items-center md:flex-row md:justify-center md:items-start">
            <!-- Contact Information - Mobile first -->
            <div class="text-center w-full md:text-start md:flex-1">
                <h2 class="text-3xl font-extrabold mb-4 md:text-4xl md:mb-6 lg:text-6xl">
                    Contact us
                </h2>
                <div class="space-y-2 text-lg md:text-xl lg:text-3xl">
                    <p>Kyiv, Ukraine</p>
                    <p>
                        <a
                            href="mailto:mail@isa.co.ua"
                            class="hover:underline transition-all duration-200 text-white"
                        >
                            mail@isa.co.ua
                        </a>
                    </p>
                </div>
                <div
                    class="flex items-center justify-center gap-4 text-xl mt-6 md:justify-start md:gap-5 md:text-2xl"
                >
                    <a href="#" aria-label="YouTube" class="hover:text-white/80">
                        <font-awesome-icon :icon="['fab', 'youtube']" />
                    </a>
                    <a href="#" aria-label="Instagram" class="hover:text-white/80">
                        <font-awesome-icon :icon="['fab', 'instagram']" />
                    </a>
                    <a href="#" aria-label="Facebook" class="hover:text-white/80">
                        <font-awesome-icon :icon="['fab', 'facebook']" />
                    </a>
                    <a href="#" aria-label="Patreon" class="hover:text-white/80">
                        <font-awesome-icon :icon="['fab', 'patreon']" />
                    </a>
                    <a href="#" aria-label="Telegram" class="hover:text-white/80">
                        <font-awesome-icon :icon="['fab', 'telegram']" />
                    </a>
                </div>
            </div>

            <!-- Logo - Hidden on mobile, visible on desktop -->
            <div ref="logoRef" >
                <img
                    src="@/assets/images/62647f9fbe07235ba9b28d67_star-white-with-animals.svg"
                    alt="ISA Logo"
                    class="w-full h-auto "
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
    // Only animate logo on desktop
    if (window.innerWidth >= 768 && logoRef.value) {
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
    
    // Handle window resize
    window.addEventListener('resize', setupAnimations);
});

onBeforeUnmount(() => {
    ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
    window.removeEventListener('resize', setupAnimations);
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

a[href^="mailto:"] {
    display: inline-block;
    transition: transform 0.2s ease, text-decoration 0.2s ease;
}

a[href^="mailto:"]:hover {
    text-decoration: underline;
    transform: translateY(-1px);
}

/* Social icons hover effect */
a:not([href^="mailto:"]) {
    transition: opacity 0.2s ease;
}

a:not([href^="mailto:"]):hover {
    opacity: 0.8;
}

/* Logo animation base state */
img {
    transform: translateY(20px);
}

/* Mobile-specific adjustments */
@media (max-width: 767px) {
    .flex-col {
        align-items: center;
    }
    
    .text-center {
        text-align: center;
    }
    
    .justify-center {
        justify-content: center;
    }
}
</style>