<template>
  <div>
    <PageContainer>
      <RulesPage />
    </PageContainer>
    <PageContainer>
      <CharactersPage />
    </PageContainer>
    <PageContainer>
      <CharacterPage />
    </PageContainer>
  </div>
</template>

<script>
import PageContainer from "@/components/PageContainer.vue";

export default {
  components: {
    PageContainer,
    RulesPage: defineAsyncComponent(() => import("./rules.vue")),
    CharactersPage: defineAsyncComponent(() => import("./characters.vue")),
    CharacterPage: defineAsyncComponent(() => import("./character.vue")),
  },
};
</script>
