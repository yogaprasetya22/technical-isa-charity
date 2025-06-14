<template>
    <div class="relative">
        <!-- Tombol Donate yang diperbarui -->
        <div
            class="donate-button bg-white "
            @click="openDonate"
        >
            <div class="donate-button-inner p-2">
                <span class="font-bold text-lg">Donate</span>
            </div>
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
    window.open("https://isa-charity.webflow.io/donate", "_blank");
}
</script>

<style scoped>
.donate-button {
    position: fixed;
    right: 1.2rem;
    top: 1.2rem;
    cursor: pointer;
    z-index: 50;
}

.donate-button-inner {
    color: black;
    border: 2px solid black;
    transition: all 0.3s ease;
    will-change: transform;
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
    text-align: center;
    font-family: sans-serif;
}

.donate-button:hover .donate-button-inner {
    transform: translate(0.5em, -0.5em);
}

</style>
