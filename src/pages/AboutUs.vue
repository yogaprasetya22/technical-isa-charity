<template>
    <div class="relative">
        <div
            class="fixed top-4 right-4 z-50 border-2 border-black p-2 bg-white text-black rounded-lg shadow-lg hover:bg-gray-100 transition-colors duration-300 supporter-button"
            @click="openDonate"
        >
            <div class="supporter-button-inner">Donate</div>
        </div>

        <HeaderSection />
        <Hero />
        <ProjectsSection />
        <HelpSection />
        <Team />
        <Supporters />
        <Partners />
        <FooterSection />
    </div>
</template>

<script setup lang="ts">
import HeaderSection from "../components/HeaderSection.vue";
import ProjectsSection from "../components/ProjectsSection.vue";
import HelpSection from "../components/HelpSection.vue";
import Team from "../components/Team.vue";
import Supporters from "../components/Supporters.vue";
import Partners from "../components/Partners.vue";
import FooterSection from "../components/FooterSection.vue";
import Hero from "../components/Hero.vue";

function openDonate() {
    window.open("https://www.patreon.com/isa_ua", "_blank");
}
</script>

<style scoped>
.supporter-button {
    position: relative;
    display: inline-block;
    cursor: pointer;
    overflow: visible;
}

.supporter-button-inner {
    background-color: white;
    color: black;
    border: 2px solid black;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
    transition: transform 0.3s ease, background-color 0.3s ease;
    will-change: transform;
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
}

.supporter-button:hover .supporter-button-inner {
    transform: translate(0.5em, -0.5em);
    box-shadow: 8px 8px 20px rgba(0, 0, 0, 0.1);
    background-color: #f5f5f5;
}
</style>
