<template>
  <div>

    <div class="icons">
      <desktop-icon icon="/icons/rules.svg" targetPage="rules" @icon-click="addPageContainer" text="Rules"/>
      <desktop-icon icon="/icons/file.svg" targetPage="characters" @icon-click="addPageContainer" text="Characters"/>
      <desktop-icon icon="/icons/user.svg" targetPage="character" @icon-click="addPageContainer" text="Character"/>
    </div>

    <div class="page-containers">
      <PageContainer v-for="(container, index) in pageContainers" :key="index" :title="container.page" @close="removePageContainer(index)">
        <component :is="getPageComponent(container.page)" />
      </PageContainer>
    </div>
  </div>
</template>

<script>
import PageContainer from "@/components/PageContainer.vue";
import DesktopIcon from "@/components/desktopIcon.vue";
import { defineAsyncComponent } from "vue";

export default {
  components: {
    PageContainer,
    DesktopIcon,
    RulesPage: defineAsyncComponent(() => import("./rules.vue")),
    CharactersPage: defineAsyncComponent(() => import("./characters.vue")),
    CharacterPage: defineAsyncComponent(() => import("./character.vue")),
  },
  data() {
    return {
      pageContainers: [], 
    };
  },
  methods: {
    addPageContainer(page) {
      this.pageContainers.push({ page });
    },
    getPageComponent(page) {
      const pageMap = {
        rules: "RulesPage",
        characters: "CharactersPage",
        character: "CharacterPage",
      };
      return pageMap[page] || null;
    },
    removePageContainer(index) {
      this.pageContainers.splice(index, 1);
    },
  },
};
</script>

<style>
.icons {
  display: flex;
  gap: 16px;
  margin-bottom: 16px;
}
.page-containers {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
</style>