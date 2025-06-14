<template>
    <div class="">
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

<script lang="ts">
import { defineComponent } from "vue";
import HeaderSection from "../components/HeaderSection.vue";
import ProjectsSection from "../components/ProjectsSection.vue";
import HelpSection from "../components/HelpSection.vue";
import Team from "../components/Team.vue";
import Supporters from "../components/Supporters.vue";
import Partners from "../components/Partners.vue";
import FooterSection from "../components/FooterSection.vue";
import Hero from "../components/Hero.vue";

export default defineComponent({
    components: {
        HeaderSection,
        Hero,
        ProjectsSection,
        HelpSection,
        Team,
        Supporters,
        Partners,
        FooterSection,
    },
});
</script>
