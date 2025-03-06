<script setup lang="ts">
import {
  Content,
  fetchOneEntry,
  isPreviewing,
  type BuilderContent,
  getBuilderSearchParams,
  fetchEntries,
} from '@builder.io/sdk-vue';
import { onMounted, ref } from 'vue';

import HelloWorldComponent from './components/HelloWorld.vue';
import PartnersCard from './components/PartnersCard.vue';

// Register your Builder components
const REGISTERED_COMPONENTS = [
  {
    component: HelloWorldComponent,
    name: 'Hello World',
    canHaveChildren: true,
    inputs: [
      {
        name: 'text',
        type: 'string',
        defaultValue: 'World',
      },
    ],
  },
  {
    component: PartnersCard,
    name: 'PartnerCard',
    inputs: [
      {
        name: "partners",
        type: "list",
        subFields: [
          {
            name: "partner",
            type: "reference",
            model: "push-partner",
          },
        ],
      },
    ],
  },
];

// TODO: enter your public API key
const BUILDER_PUBLIC_API_KEY = 'db60bf3db7fa4db7be81ef05b72bd720'; // ggignore
const content = ref<BuilderContent | null>(null);
const allContent = ref<BuilderContent[]>([]);
const canShowContent = ref(false);
const model = 'page';

// onMounted(async () => {
//   content.value = await fetchOneEntry({
//     model,
//     apiKey: BUILDER_PUBLIC_API_KEY,
//     options: getBuilderSearchParams(new URL(location.href).searchParams),
//     userAttributes: {
//       urlPath: window.location.pathname,
//     },
//     enrich: true
//   });
//   canShowContent.value = content.value ? true : isPreviewing();
// });

async function fetchContent() {
  content.value = await fetchOneEntry({
    model,
    apiKey: BUILDER_PUBLIC_API_KEY,
    options: getBuilderSearchParams(new URL(location.href).searchParams),
    userAttributes: {
      urlPath: window.location.pathname,
    },
    enrich: true,
  });
  canShowContent.value = content.value ? true : isPreviewing();
}

fetchContent();

async function fetchAllContent() {
  try {
    const response = await fetchEntries({
      model,
      apiKey: BUILDER_PUBLIC_API_KEY,
      locale: 'en-US',
      options: { noTargeting: true }
    });

    console.log('Fetched Content:', response); // Log the response
    allContent.value = response;
  } catch (error) {
    console.error('Error fetching content:', error);
  }
}

fetchAllContent();


</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
  </header>
  <div>
    <div>Hello world from your Vue 3 project. Below is Builder Content:</div>
    <div v-if="canShowContent">
      <div>
        page title:
        {{ (content && content.data && content.data.title) || 'Unpublished' }}
      </div>
      <Content :model="model" :content="content" :api-key="BUILDER_PUBLIC_API_KEY"
        :customComponents="REGISTERED_COMPONENTS" :enrich="true" :data="{ name: 'My HTML Attribute text' }" />
    </div>
    <div v-else>Content not Found</div>
  </div>
</template>

<style>
#app {
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}
</style>
