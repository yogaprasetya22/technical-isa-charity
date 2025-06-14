<template>
    <div class="p-[10%] mx-auto w-full bg-white">
        <h2 class="text-xl font-bold mb-4 text-[8vw] md:text-[4vw]">We are</h2>
        <p class="mb-[1em] mt-[.5em] text-[3.75vw] md:text-[2.5vw]">
            team of like-minded people, socially active companies, media
            personalities, volunteers who help low-income shelters and foster
            carers to support homeless animals.
        </p>
        <h3 class="text-xl font-bold mb-4 text-[8vw] md:text-[4vw]">Our primary goal</h3>
        <p class="mb-[1em] mt-[.5em] text-[3.75vw] md:text-[2.5vw]">
            is to provide animals with the most necessary things - food,
            medicine, and urgent needs for the livelihood of shelters. We
            believe in our power and the power of conscious citizens who care
            about the environment and their health, who understand the
            importance of helping each other, especially our little friends.
            Growth begins with your care for your loved ones. Animals are one of
            the closest creatures to humans who remain with us throughout our
            lives. We all know the healing power of warmth, grace, and big
            loving eyes.
        </p>
        <blockquote
            class="text-black text-left font-dm-sans text-[3.75vw] md:text-[2.5vw] leading-[1.5em] block border-l-4 border-[#ff97d0] mt-0 mb-0  ml-4 md:ml-16 p-4 italic font-normal"
        >
            "Animals need to have friends. Just like humans."
            <br />
            <span class="block mt-2"
                >- James Herriot, All Creatures Great and Small</span
            >
        </blockquote>
        <p class="mb-[1em] mt-[.5em] text-[3.75vw] md:text-[2.5vw]">
            Who else if not we should support our younger brothers, especially
            in difficult times? Let's be friends!
        </p>
        <div class="mt-6" ref="videoSection">
            <div class="video-wrapper">
                <iframe
                    ref="videoIframe"
                    class="w-full aspect-video"
                    src="https://www.youtube.com/embed/5Wk1rp99B7o?enablejsapi=1"
                    title="YouTube video"
                    frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                    allowfullscreen
                ></iframe>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

// Refs
const videoSection = ref<HTMLElement | null>(null);
const videoIframe = ref<HTMLIFrameElement | null>(null);

// Animation setup
const setupVideoAnimation = () => {
    if (!videoSection.value || !videoIframe.value) return;

    // Set initial state - HANYA jika element belum terlihat
    if (videoSection.value.getBoundingClientRect().top > window.innerHeight) {
        gsap.set(videoSection.value, {
            opacity: 0,
            y: 100,
            scale: 0.95,
            rotation: 1,
        });

        gsap.set(videoIframe.value, {
            boxShadow: "0 0 0 rgba(0,0,0,0)",
        });
    }

    // Animasi untuk scroll ke BAWAH
    ScrollTrigger.create({
        trigger: videoSection.value,
        start: "top 80%",
        end: "top 30%",
        onEnter: () => animateVideoIn(),
        onLeaveBack: () => animateVideoOut(),
    });

    // Animasi untuk scroll ke ATAS
    ScrollTrigger.create({
        trigger: videoSection.value,
        start: "top 80%",
        end: "bottom 50%",
        onEnterBack: () => animateVideoIn(),
        onLeave: () => animateVideoOut(),
    });

    // Fungsi animasi masuk
    const animateVideoIn = () => {
        gsap.killTweensOf([videoSection.value, videoIframe.value]);

        const tl = gsap.timeline();
        tl.to(videoSection.value, {
            opacity: 1,
            y: 0,
            scale: 1,
            rotation: 0,
            duration: 1.2,
            ease: "elastic.out(1, 0.5)",
        })
            .to(
                videoIframe.value,
                {
                    boxShadow: "0 25px 50px -10px rgba(0,0,0,0.3)",
                    duration: 0.8,
                    ease: "power3.out",
                },
                "-=0.5"
            )
            .fromTo(
                videoIframe.value,
                { scale: 0.98 },
                {
                    scale: 1,
                    duration: 0.6,
                    ease: "back.out(2)",
                },
                "-=0.3"
            );
    };

    // Fungsi animasi keluar
    const animateVideoOut = () => {
        gsap.killTweensOf([videoSection.value, videoIframe.value]);

        const tl = gsap.timeline();
        tl.to(videoIframe.value, {
            boxShadow: "0 0 0 rgba(0,0,0,0)",
            duration: 0.4,
            ease: "power3.in",
        }).to(
            videoSection.value,
            {
                opacity: 0,
                y: 100,
                scale: 0.95,
                rotation: 1,
                duration: 0.8,
                ease: "power3.inOut",
            },
            "-=0.2"
        );
    };
};

// Lifecycle hooks
onMounted(() => {
    gsap.registerPlugin(ScrollTrigger);
    // Tambahkan timeout kecil untuk memastikan DOM selesai render
    setTimeout(() => {
        setupVideoAnimation();
    }, 100);
});

onBeforeUnmount(() => {
    ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
});
</script>

<style scoped>
.video-wrapper {
    position: relative;
    overflow: hidden;
    border-radius: 12px;
    transform-style: preserve-3d;
    perspective: 1000px;
    will-change: transform, opacity;
    background: #f0f0f0; /* Fallback background */
    min-height: 300px; /* Pastikan ada tinggi minimum */
}

.video-wrapper::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
        45deg,
        rgba(255, 151, 208, 0.2) 0%,
        rgba(252, 249, 68, 0.1) 100%
    );
    z-index: 1;
    opacity: 0;
    transition: opacity 0.6s ease;
}

.video-wrapper:hover::before {
    opacity: 1;
}

iframe {
    display: block;
    width: 100%;
    height: 100%;
    min-height: 300px;
    border: none;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    transform-origin: center center;
    backface-visibility: hidden;
    will-change: transform, box-shadow;
}

/* Pastikan aspect ratio tetap terjaga */
.aspect-video {
    aspect-ratio: 16/9;
}
</style>
