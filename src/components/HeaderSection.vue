<template>
    <section
        ref="headerRef"
        class="bg-[#fcf944] text-black min-h-screen flex items-center justify-center relative overflow-hidden"
    >
        <div
            class="flex flex-col items-center justify-center text-center px-4 w-full"
        >
            <div
                class="flex flex-row gap-5 md:gap-10 w-full justify-center items-center"
            >
                <!-- Animated Logo -->
                <div class="relative w-20 h-20 md:w-56 md:h-56 mb-6">
                    <img
                        ref="logoInRef"
                        :src="currentLogo"
                        class="absolute inset-0 w-full h-full object-contain logo logo-in"
                        alt="Current logo"
                    />
                    <img
                        ref="logoOutRef"
                        :src="previousLogo"
                        class="absolute inset-0 w-full h-full object-contain logo logo-out"
                        alt="Previous logo"
                    />
                </div>

                <!-- Title -->
                <div
                    class="font-extrabold text-2xl sm:text-4xl md:text-5xl leading-tight text-start hero-line"
                >
                    <h1>Innovative</h1>
                    <h1>Solutions</h1>
                    <h1>for</h1>
                    <h1>Animals</h1>
                </div>
            </div>

            <!-- Subheading -->
            <p class="subtitle text-sm md:text-lg mt-4 tracking-wide lowercase">
                charity organization
            </p>

            <!-- Social Icons -->
            <div
                class="socials flex items-center justify-center gap-5 text-2xl mt-6"
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
    </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { library } from "@fortawesome/fontawesome-svg-core";
import {
    faYoutube,
    faInstagram,
    faFacebook,
    faPatreon,
    faTelegram,
} from "@fortawesome/free-brands-svg-icons";

// Register Font Awesome icons
library.add(faYoutube, faInstagram, faFacebook, faPatreon, faTelegram);

// GSAP plugins registration
gsap.registerPlugin(ScrollTrigger);

// Refs
const headerRef = ref<HTMLElement | null>(null);
const logoInRef = ref<HTMLImageElement | null>(null);
const logoOutRef = ref<HTMLImageElement | null>(null);

// Logo animation
const logos = [
    new URL(
        "@/assets/images/627fc7fcdc0f8e9cbcc8ffed_isa-logo-dog-clean.svg",
        import.meta.url
    ).href,
    new URL(
        "@/assets/images/627fc81139e6f5dca2d02054_isa-logo-cat-clean.svg",
        import.meta.url
    ).href,
];

const currentIndex = ref(0);
const currentLogo = ref(logos[currentIndex.value]);
const previousLogo = ref(currentLogo.value);
let logoInterval: number;

const animateLogo = () => {
    const nextIndex = (currentIndex.value + 1) % logos.length;
    const logoIn = logoInRef.value;
    const logoOut = logoOutRef.value;

    if (!logoIn || !logoOut) return;

    previousLogo.value = currentLogo.value;
    currentLogo.value = logos[nextIndex];

    gsap.set(logoOut, { y: 0, opacity: 1 });
    gsap.set(logoIn, { y: "-100%", opacity: 1 });

    const tl = gsap.timeline();
    tl.to(logoOut, {
        y: "100%",
        opacity: 0,
        duration: 0.4,
        ease: "power1.inOut",
    }).to(
        logoIn,
        {
            y: "0%",
            opacity: 1,
            duration: 0.4,
            ease: "power1.inOut",
        },
        "<"
    );

    currentIndex.value = nextIndex;
};

const initAnimations = () => {
    // Text animations - exactly matching your original timing
    gsap.from(".hero-line h1", {
        opacity: 0,
        y: 20,
        stagger: 0.1,
        duration: 0.5,
        ease: "power2.out",
        delay: 0.3,
    });

    gsap.from(".subtitle", {
        opacity: 0,
        y: 20,
        duration: 0.6,
        ease: "power2.out",
        delay: 0.8,
    });

    gsap.from(".socials a", {
        opacity: 0,
        y: 20,
        stagger: 0.15,
        duration: 0.5,
        ease: "power2.out",
        delay: 1.0,
    });

    // Start logo animation
    animateLogo();
    logoInterval = window.setInterval(animateLogo, 3000);
};

const setupScrollTrigger = () => {
    ScrollTrigger.create({
        trigger: headerRef.value,
        start: "top 80%",
        end: "bottom 50%",
        onEnterBack: () => {
            // Replay animations when scrolling back up
            gsap.from(".hero-line h1", {
                opacity: 0,
                y: 20,
                stagger: 0.1,
                duration: 0.8,
                ease: "power2.out",
            });

            gsap.from(".subtitle", {
                opacity: 0,
                y: 20,
                duration: 0.8,
                ease: "power2.out",
            });

            gsap.from(".socials a", {
                opacity: 0,
                y: 20,
                stagger: 0.15,
                duration: 0.8,
                ease: "power2.out",
            });
        },
    });
};

onMounted(() => {
    initAnimations();
    setupScrollTrigger();
});

onBeforeUnmount(() => {
    if (logoInterval) clearInterval(logoInterval);
    gsap.killTweensOf(".hero-line h1, .subtitle, .socials a");
    ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
});
</script>

<style scoped lang="scss">
.logo {
    transition: none;

    &-in,
    &-out {
        @apply absolute inset-0 w-full h-full object-contain;
    }
}

.hero-line {
    @apply block;
}

.subtitle {
    @apply uppercase;
}

.socials a {
    transition: transform 0.3s ease;

    &:hover {
        transform: translateY(-3px);
    }
}
</style>
